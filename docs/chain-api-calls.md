# Chain api calls to retrieve information

The follow steps are required to retrieve metadata or files from multiple items:
- Create a list of item ids
- Loop through the list and gather the information needed for each item id 

## Code to retrieve item ids by:
### Search query
This example searches for records with that use the category 'Digital Humanities' or 'Environmental Humanities'

'''py
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

#Create a dataframe from the JSON formatted data
df_results = pd.DataFrame(results)
df = df_results.drop_duplicates(subset='id', keep="first")
print(len(df)-len(df_results),'records removed,',len(df),'unique records remain')
#Create a list of all the item ids
item_ids = df['id'].tolist()
#See the number of ids
print(len(article_ids),'ids in list')
'''

### An account
### A group

## Code to gather additional info based on a list of item ids

### Full metadata
### Views and Downloads, timeline, locations
### Download file(s)
