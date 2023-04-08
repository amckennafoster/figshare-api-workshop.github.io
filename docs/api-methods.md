---
layout: lesson
---

## Summary of GET, POST, PUT, DELETE methods 

## Example code
### Return results (GET)

```py
import json
import requests
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
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Retrieve basic metadata for 10 items your account owns
r=requests.get(BASE_URL + '/account/articles/?page=1&page_size=10', headers=api_call_headers)
#Load the metadata as JSON
metadata=json.loads(r.text)
#View the metadata
print(metadata)
```

### Send parameters (POST, PUT)

```py
import json
import requests
#Set the base URL and item id
BASE_URL = 'https://api.figshare.com/v2'
ITEM_ID = 123456
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Create json formatted for upload
sample_metadata = {"title":"Test metadata for upload","keywords":["biodiversity","invertebrate"]}
json_metadata = json.dumps(sample_metadata)
#Create a private item
r = requests.post(BASE_URL + '/account/articles', headers=api_call_headers, data = json_metadata)
if r.status_code == 201: #If Post was successful
  print('Successfully created item')
```

### Impersonate (GET, POST, PUT, DELETE)

```py
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header -this must be from an administrator account for impersonation
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Get the author info from this endpoint: https://docs.figshare.com/#private_institution_accounts_list
account_id = ENTER ACCOUNT ID #This is 'id' in the endpoint output and is the account id for impersonation
ITEM_ID = 123456
#Create json formatted for upload
sample_metadata = {"title":"Test metadata for upload","keywords":["biodiversity","invertebrate"]}
json_metadata = json.dumps(sample_metadata)
json_metadata["impersonate"] = account_id #Adding this to the metadata for upload enables impersonation
#Create a private item in the user account
r = requests.post(BASE_URL + '/account/articles' + str(ITEM_ID), headers=api_call_headers, data = json_metadata)
if r.status_code == 201: #If Post was successful
  print('Successfully created item')
```

