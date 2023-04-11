---
layout: lesson
---

# Chain api calls to retrieve information

The follow steps are required to retrieve metadata or files from multiple items:
- Create a list of item ids
- Loop through the list and gather the information needed for each item id 

The code below loops through a list of item ids using [for loops](https://wiki.python.org/moin/ForLoop).

[Retrieve item ids in your account](#retrieve-item-ids-in-your-account)
[Retrieve item ids through a search query](#retrieve-item-ids-through-a-search-query)
[Now gather inforamtion for those item ids](#now-gather-inforamtion-for-those-item-ids)
 - [Full metadata](#full-metadata)
 - [Views and Downloads](#views-and-downloads)
 - [Download file(s)](#download-files)

## First retrieve item ids

### Retrieve item ids in your account
```py
import json
import requests
#Retrieve list of private metadata for 50 items. This is for unpublished and published records.
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Get items owned by account
r=requests.get(BASE_URL + '/account/articles?page=1&page_size=50', headers=api_call_headers) 
items=json.loads(r.text)
if r.status_code != 200:
    print('Something is wrong:',r.content)
else:
    print('Collected',len(items),'metadata records')
#Create a list of all the item ids
item_ids = [item['id'] for item in items]  
```

### Retrieve item ids through a search query
This example searches for records with that use the category 'Digital Humanities' or 'Environmental Humanities'

```py
import json
import requests
categories = ["'Digital Humanities'","'Environmental Humanities'"]
#Gather basic metadata for items (articles) that meet your search criteria
results = [] #create a blank list
for i in categories:
    query = '{"search_for":":category: ' + i + '"}'
    y = json.loads(query) #Figshare API requires json paramaters
    #The number of results is unknown but you can collect up to 9,000 results. This sets the page size to 1000 results and calls the API 
    #to retrieve 9 pages of results
    for j in range(1,10):
        records = json.loads(requests.post(BASE_URL + '/articles/search?page_size=1000&page={}'.format(j), params=y).content)
        results.extend(records) #add the retrieved records to the list of records
#See the number of items
print(len(results),'items retrieved')

#Create a list of all the item ids
item_ids = [item['id'] for item in published_records]  
#Remove duplicates by converting to a dictionary and back to a list
item_ids_unique = list( dict.fromkeys(item_ids) ) 
print(len(item_ids)-len(item_ids_unique),'duplicate records removed,',len(item_ids_unique),'unique records remain')
```

### Retrieve item ids in a group ???

## Now gather inforamtion for those item ids

### Full metadata
Use one of the methods above to create a list of item ids called item_ids. This API call includes a token in the header in case some of the item ids are for unpublished records

```py
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}

#---INSERT CODE TO COLLECT ITEM IDS HERE----

full_records = [] #Create a blank list to hold the JSON metadata records
for id_value in item_ids: 
    r=requests.get(BASE_URL + '/account/articles/' + str(id_value), headers=api_call_headers)
    metadata=json.loads(r.text)
    full_records.append(metadata) #Add the collected metadata record to the master list of records

print(len(full_records),metadata records collected)
```

### Views and Downloads

To be added

### Download file(s)

There are several ways to download files through the API. Each file that is part of a record has it's own download URL. You can find this URL in the full metadata retrieved through the API or you can enter the item id at this endpoint: [https://docs.figshare.com/#article_files](https://docs.figshare.com/#article_files). For item [https://doi.org/10.6084/m9.figshare.5616409.v3](https://doi.org/10.6084/m9.figshare.5616409.v3), the download URL for the file is [https://figshare.com/ndownloader/files/9778696](https://figshare.com/ndownloader/files/9778696). Visiting that URL will automatically start the download (*File is 219KB*).

Python:
```py
import json
import requests
from pathlib import Path

#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'

file_info = [] #a blank list to hold all the file metadata

for i in item_ids:
    r = requests.get(BASE_URL + '/articles/' + str(i) + '/files')
    file_metadata = json.loads(r.text)
    for j in file_metadata: #add the item id to each file record- this is used later to name a folder to save the file to
        j['item_id'] = i
        file_info.append(j) #Add the file metadata to the list

#Download each file to a subfolder named for the article id and save with the file name
for k in file_info:
    response = requests.get(BASE_URL + '/file/download/' + str(k['id']), headers=api_call_headers)
    Path(str(k['item_id'])).mkdir(exist_ok=True)
    open(str(k['item_id']) + '/' + k['name'], 'wb').write(response.content)
    
print('All done.')
```
Output: The download script will save files to folders named for the item id the files belong to
![picture of a folder window with two folders of downloaded files](download-folders.jpg)
