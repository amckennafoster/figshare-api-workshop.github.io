---
layout: resource
---

# Impersonating user accounts

Impersonating accounts is described in [this section](https://docs.figshare.com/#figshare_documentation_api_description_impersonation) of the documentation. Adding 'impersonate=*account_id*' to either the request URL for GET or in the body of a POST or PUT request will impersonate the account indicated. Here is a brief exaple that changes the author on an item owned by a user account and then publishes the item:
```py
import json
import requests

#Set the token in the header and base URL
text_file = open("./././testing-token.txt", "r") #Paste your token in a text file and save it where this notebook is
TOKEN = text_file.read()
TOKEN.strip() #removes any hidden spaces
text_file.close()


api_call_headers = {'Authorization': 'token ' + TOKEN}

#Set the base URL
BASE_URL = 'https://api.figshare.com/v2' #Change this to 'https://api.figsh.com/v2' if you want to test in stage

#Get the author info from this endpoint: https://docs.figshare.com/#private_institution_accounts_list
new_user_id = 12345 #this is the user_id in the endpoint output
account_id = 6789 #This is 'id' in the endpoint output and is the account id for impersonation

#Set up the json to replace the author(s)
authors = {'authors': [{'id': new_user_id}], 'impersonate': account_id}
author_json = json.dumps(authors)

#Store the item id to be edited (you could also loop through a list of ids)
item_id = 65432 
    
s = requests.put(BASE_URL + '/account/articles/' + str(item_id), headers=api_call_headers, data = author_json) 
if s.status_code != 205:
	print(str(s.content)) #print error
else:
	#Publish the record
    body = '{"impersonate":' + str(account_id) + '}' #impersonate to publish
    u = requests.post(BASE_URL + '/account/articles/' + str(i) +'/publish', headers=api_call_headers, data = body)
    if u.status_code != 201:
        print(str(u.content)) print error
    else:
        print('Published!')
```