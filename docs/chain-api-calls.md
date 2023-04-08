# Chain api calls to retrieve information

The follow steps are required to retrieve metadata or files from multiple items:
- Create a list of item ids
- Loop through the list and gather the information needed for each item id 

## Code to retrieve item ids by:
### Search query
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

### Your account
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
item_ids = [item['id'] for item in published_records]  
```

### A group

## Code to gather additional info based on a list of item ids

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
```

### Views and Downloads, timeline, locations
### Download file(s)
