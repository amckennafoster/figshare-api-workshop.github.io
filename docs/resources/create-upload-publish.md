---
layout: resource
---

# Create records, upload files, and publish

A common use of the API is to create metadata records from existing metadata. For example you may have metadata harvested from somewhere else that you want to include in your repository as metadata only or linked records. Or you may have metadata and files that you want to add to an account or repository. This section will assume you have already formatted the metadata for upload.

## Create a metadata only or linked file record
To create records using the API, you need to use three endpoints: 1) <a href="https://docs.figshare.com/#private_article" target="_blank">create a private record</a>, optionally <a href="https://docs.figshare.com/#upload_files_steps_to_upload_file" target="_blank">add a link to a file</a>, and then <a href="https://docs.figshare.com/#private_article" target="_blank">publish the record</a>.

This Python example will create a linked file record for a paper- effectively it is a metadata only record that points users to the publisher's DOI.

```py
import json
import requests

#Set the token in the header and base URL
#Open a token stored in a text document:
text_file = open("symp-figsh-token.txt", "r") #Change this file name
TOKEN = text_file.read()
TOKEN.strip() #removes any hidden spaces
text_file.close()

#Or add your token as plain text (if you do, don't share this script with anyone):
#api_call_headers = {'Authorization': 'token ' + TOKEN}

#Set the base URL
BASE_URL = 'https://api.figshare.com/v2' #To use stage instance replace with 'https://api.figsh.com/v2'

#Create a json file with 1 record
jsonfile = {
  "title": "Producing performance-advantaged bioplastics",
  "description": "Abstract: A grand challenge for bio-based plastics is the ability to cost-effectively manufacture high-performance polymers directly from renewable resources that are also recyclable-by-design. A one-step conversion of xylose to polyesters has been reported, combining a sustainable lifecycle with impressive materials performance.",
  "tags": [
    "bioplastics"
  ],
  "categories": [
      614,
	  1058
  ],
  "authors": [
    {
	"name":"Robin M. Cywar"
	},
	{
      "id": "Gregg Beckham"
    }
  ],
  "defined_type": "journal contribution",
  "license": 1,
  "doi": "",
  "handle": "",
  "resource_doi": "10.1038/s41557-022-01030-y",
  "resource_title": "Producing performance-advantaged bioplastics",
    "timeline": {
    "firstOnline": "2022-11-26"
  },
  "funding": "National Renewable Energy Laboratory",
  "custom_fields": {
    "Journal / Parent title": "Nature Chemistry",
	"Volume": "14",
	"Pagination":"967-969"
  }
}

#You can also open a json file if you prefer:
#with open("beckham-record.json", "r", encoding='utf8') as read_file: #Replace this with the filename of your choice
#    jsonfile = json.load(read_file)

#Upload the record

record_fails = []

jsonresult = json.dumps(jsonfile) #Takes one record and makes it a json string (double quotes)
r = requests.post(BASE_URL + '/account/articles', headers=api_call_headers, data = jsonresult)
if r.status_code != 201:
    record_fails.append(str(r.content[0:75])) #Add failed index to list with partial description
else:

    #Remove the admin account as an author by updating the record just created
    #This uses the article url returned by the API response (r)
    authordict = {}
    authordict['authors'] = jsonfile['authors']
    authorjson = json.dumps(authordict) #formats everything with double quotes
    response_json = json.loads(r.content)
    new_url = response_json['location']
    s = requests.put(new_url, headers=api_call_headers, data = authorjson) 
    
    #Upload a link as a file
    link = '{"link":"https://doi.org/'+ jsonfile['resource_doi'] +'"}'
    tup = new_url.rpartition('/') #From end of URL, split string '/' so we can get the article id
    article_id = str(tup[2]) #Then select the id from the resulting tuple.
    t = requests.post(BASE_URL + '/account/articles/' + str(article_id) +'/files', headers=api_call_headers, data = link)

    #Publish the record
    u = requests.post(BASE_URL + '/account/articles/' + str(article_id) +'/publish', headers=api_call_headers)
    
        
#print(len(record_fails),"record fails. Failed record indexes:",record_fails)
print('The record is created and available here:',json.loads(u.content)['location'])
```

## Upload files
The process to upload files may seem complex but this is necessary to handle extremely large files. So whether your file is a few kilobytes or a terabyte, this process will work. 

The overall process to upload files looks like this: 1) <a href="https://docs.figshare.com/#private_article" target="_blank">create a private record</a>, <a href="https://docs.figshare.com/#upload_files_steps_to_upload_file" target="_blank">upload file(s)</a>, and then <a href="https://docs.figshare.com/#private_article" target="_blank">publish the record</a>.

As described in <a href="https://docs.figshare.com/#upload_files_steps_to_upload_file" target="_blank">the upload documentation</a>, there are four steps to uploading: 1) Initiate the upload 2) Receive the number of file parts, 3) Upload the file parts, and 4) Complete the upload. The documentation linked above has a sample script in several languages. A Jupyter Notebook and Python example is downloadable from <a href="https://colab.research.google.com/drive/13CAM8mL1u7ZsqNhfZLv7bNb1rdhMI64d?usp=sharing" target="_blank">this Google Colab file</a> and is reproduced below:

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

## Upload metadata and files in batch

If your existing metadata includes a URL or local location for the associated file(s) you can combine the two example scripts on this page to upload multiple records with their files. Your existing metadata should be a list of dictionaries already formatted for upload (see the <a href="https://amckennafoster.github.io/figshare-api-workshop.github.io/resources/formatting.html" target="_blank">formatting page</a> for advice). Use a for loop to upload the metadata for each record, then upload the file(s).
