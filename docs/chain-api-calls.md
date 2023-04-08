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
#See the number of articles
print(len(results),'articles retrieved')

#Create a list of all the article ids
item_ids = [item['id'] for item in published_records]  
#Remove duplicates by converting to a dictionary and back to a list
item_ids_unique = list( dict.fromkeys(item_ids) ) 
print(len(item_ids)-len(item_ids_unique),'duplicate records removed,',len(item_ids_unique),'unique records remain')
```

### An account
### A group

## Code to gather additional info based on a list of item ids

### Full metadata
### Views and Downloads, timeline, locations
### Download file(s)
