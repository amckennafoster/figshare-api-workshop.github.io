---
layout: workshop-lesson
---

# Programmatically Creating or Managing Records

A common use of the API is to create metadata records from existing metadata. For example you may have metadata harvested from somewhere else that you want to include in your repository as metadata only or linked records. This section will assume you have already formatted the metadata for upload.

## Create metadata only or linked file record
To create records using the API, you need to use three endpoints: 1) <a href="https://docs.figshare.com/#private_article" target="_blank">create a private record</a>, optionally <a href="https://docs.figshare.com/#upload_files_steps_to_upload_file" target="_blank">add a link to a file</a>, and then <a href="https://docs.figshare.com/#private_article" target="_blank">publish the record</a>.

## Upload files
The process to upload files may seem complex but this is necessary to handle extremely large files. So whether your file is a few kilobytes or a terabyte, this process will work. 

As described in <a href="https://docs.figshare.com/#upload_files_steps_to_upload_file" target="_blank">the documentation</a>, there are four steps to uploading: 1) Initiate the upload 2) Receive the number of file parts, 3) Upload the file parts, and 4) Complete the upload. The documentation linked above has a sample script in several languages. A Jupyter Notebook and Python example is downloadable from <a href="https://colab.research.google.com/drive/13CAM8mL1u7ZsqNhfZLv7bNb1rdhMI64d?usp=sharing" target="_blank">this Google Colab file</a> and is reproduced below:

``` py
BASE_URL = 'https://api.figshare.com/v2/{endpoint}'
TOKEN = 'ENTER TOKEN BETWEEN QUOTES'
CHUNK_SIZE = 10485760 #about 10MB
FILE_PATH = 'FILE NAME' #If the file is in the same folder as this notebook, put the name of the file in quotes. E.g. 'file-name.mp4'
TITLE = 'Uploaded through API' #This is the title of the item you want to upload to.
#Define functions that split the file, initiate upload, and complete the upload

def raw_issue_request(method, url, data=None, binary=False):
    headers = {'Authorization': 'token ' + TOKEN}
    if data is not None and not binary:
        data = json.dumps(data)
    response = requests.request(method, url, headers=headers, data=data)
    try:
        response.raise_for_status()
        try:
            data = json.loads(response.content)
        except ValueError:
            data = response.content
    except HTTPError as error:
        print('Caught an HTTPError: {}'.format(error.message))
        print('Body:\n', response.content)
        raise

    return data


def issue_request(method, endpoint, *args, **kwargs):
    return raw_issue_request(method, BASE_URL.format(endpoint=endpoint), *args, **kwargs)


def list_articles():
    result = issue_request('GET', 'account/articles')
    print('Listing current articles:')
    if result:
        for item in result:
            print(u'  {url} - {title}'.format(**item))
    else:
        print('  No articles.')



def create_article(title):
    data = {
        'title': TITLE  # You may add any other information about the article here as you wish.
    }
    result = issue_request('POST', 'account/articles', data=data)
    print('Created article:', result['location'], '\n')

    result = raw_issue_request('GET', result['location'])

    return result['id']


def list_files_of_article(article_id):
    result = issue_request('GET', 'account/articles/{}/files'.format(article_id))
    print('Listing files for article {}:'.format(article_id))
    if result:
        for item in result:
            print('  {id} - {name}'.format(**item))
    else:
        print('  No files.')

    print


def get_file_check_data(file_name):
    with open(file_name, 'rb') as fin:
        md5 = hashlib.md5()
        size = 0
        data = fin.read(CHUNK_SIZE)
        while data:
            size += len(data)
            md5.update(data)
            data = fin.read(CHUNK_SIZE)
        return md5.hexdigest(), size


def initiate_new_upload(article_id, file_name):
    endpoint = 'account/articles/{}/files'
    endpoint = endpoint.format(article_id)

    md5, size = get_file_check_data(file_name)
    data = {'name': os.path.basename(file_name),
            'md5': md5,
            'size': size}

    result = issue_request('POST', endpoint, data=data)
    print('Initiated file upload:', result['location'], '\n')

    result = raw_issue_request('GET', result['location'])

    return result


def complete_upload(article_id, file_id):
    issue_request('POST', 'account/articles/{}/files/{}'.format(article_id, file_id))


def upload_parts(file_info):
    url = '{upload_url}'.format(**file_info)
    result = raw_issue_request('GET', url)

    print('Uploading parts:')
    with open(FILE_PATH, 'rb') as fin:
        for part in result['parts']:
            upload_part(file_info, fin, part)


def upload_part(file_info, stream, part):
    udata = file_info.copy()
    udata.update(part)
    url = '{upload_url}/{partNo}'.format(**udata)

    stream.seek(part['startOffset'])
    data = stream.read(part['endOffset'] - part['startOffset'] + 1)

    raw_issue_request('PUT', url, data=data, binary=True)
    print('  Uploaded part {partNo} from {startOffset} to {endOffset}'.format(**part))


def main():
    # We first create the article
    list_articles()
    article_id = create_article(TITLE)
    list_articles() #Prints the list again and you should see your new item at the top of the list
    list_files_of_article(article_id)

    # Then we upload the file.
    file_info = initiate_new_upload(article_id, FILE_PATH)
    # Until here we used the figshare API; following lines use the figshare upload service API.
    upload_parts(file_info)
	# We return to the figshare API to complete the file upload process.
    complete_upload(article_id, file_info['id'])
    list_files_of_article(article_id)

#Run this to upload the file.
if __name__ == '__main__':
    main()
```

## Upload metadata and files in batch.
