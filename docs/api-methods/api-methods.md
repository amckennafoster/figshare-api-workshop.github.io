---
layout: default
---

## Summary of GET, POST, PUT, DELETE methods 

## Example code
### Return results (GET)

```py
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
ITEM_ID = 123456
#Retrieve public metadata from the endpoint
s=requests.get(BASE_URL + '/articles/' + str(ITEM_ID))
#Load the metadata as JSON
metadata=json.loads(s.text)
#View the metadata
print(metadata)
```


### Authenticate (GET, POST, PUT, DELETE)

```py
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
ITEM_ID = 123456
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Retrieve private metadata from the endpoint
r=requests.get(BASE_URL + '/account/articles/' + str(ITEM_ID), headers=api_call_headers)
#Load the metadata as JSON
metadata=json.loads(r.text)
#View the metadata
print(metadata)
```

### Send parameters (POST, PUT)

```py
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
ITEM_ID = 123456
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Create json formatted for upload
sample_metadata = {"title":"Test metadata for upload","keywords":["biodiversity","invertebrate"]}
json_metadata = json.dumps(sample_metadata) #Takes one record and makes it a json string (double quotes)
#Create a private item
r = requests.post(BASE_URL + '/account/articles', headers=api_call_headers, data = json_metadata)
if r.status_code == 201: #If Post was successful
  print('Successfully created item')
```

### Impersonate (POST, PUT)

```py
// Python example
print('some example code')
```

## Code to retrieve item ids by:
### Search query
### An account
### A group

## Code to gather additional info based on a list of item ids

### Full metadata
### Views and Downloads, timeline, locations
### Download file(s)
