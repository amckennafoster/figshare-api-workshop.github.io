---
layout: resource
---

# Statistics Endpoints

The stats endpoints in Figshare provide statistics for items, authors, collections, groups, and projects. Where appropriate, the results can be filtered by category or item_type. Generally, anything from within the institution scope requires authentication. The following information is available:

1. [Top](https://docs.figshare.com/#stats_tops) views, downloads, or shares in an optionally sepcified time period. In addition to category and item_type, can also be filtered by referral (e.g. list of top referring sites by views)
2. [Total](https://docs.figshare.com/#stats_totals) views, downloads, or shares in an optionally sepcified time period
3. [Timeline](https://docs.figshare.com/#stats_timeline): Views, downloads, or shares over time (by day, month, year or total) and optionally filtered by a start_date and end_date.
4. [Geolocation](https://docs.figshare.com/#stats_breakdown): Views, downloads, or shares by country and city and optionally filtered by a start_date and end_date.
5. [Count items](https://docs.figshare.com/#stats_count_articles): Retrieve the number of items from one or more public groups.

It is important to note that **the endpoints for timeline and geolocation require a separate password** instead of your administrator token when requesting results from within the institution scope (figshare.com items need no authentication). This is due to a legacy system. Request these credentials through a support request (support@figshare.com).

Here are a few examples using python:

Retrieve total views for one item:
```py
import json
import requests
BASE_URL = 'https://stats.figshare.com'
URL = BASE_URL + '/total/views/article/21332'
r = requests.get(URL, headers=api_call_headers)
result=json.loads(r.text)
print(result)
```

Retrieve views from an institution's group by month between January 1, 2020 and May 12, 2022. This requires the username and password you received from Support and the credentials must be encoded as base64. In this example, the credentials are stored in plain text in a text file:
```py
import json
import requests
import base64
import pandas as pd

#Open credentials, set the institution string and base URL
text_file = open("team-token.txt", "r")
auth = text_file.read()
auth.strip() #removes any hidden spaces
text_file.close()
#Set the base URL and Institution name
BASE_URL = 'https://stats.figshare.com'
INST = 'ENTER repository URL base here' #Example: 'team' for https://team.figshare.com

#Encode the credentials and create the header
"#Encode the username and password
sample_string = auth
sample_string_bytes = sample_string.encode("ascii")
base64_bytes = base64.b64encode(sample_string_bytes)
api_call_headers = {'Authorization': 'Basic ' + base64_string}"

#Create the request URL
URL = BASE_URL + '/'+ INST + '/timeline/month/views/group/21332?start_date=2020-01-03&end_date=2022-05-12'
#Send the request
r = requests.get(URL, headers=api_call_headers)
#Show the request content
result=json.loads(r.text)
print(result)

#To see the results in a table
df1 = pd.json_normalize(result['timeline'])
df = df1.T
df.reset_index(inplace=True)
df = df.rename(columns = {'index':'date', 0:'value'})
df.head()
```

For suggestions and examples for reporting, see the [statistis and reporting](./statistics-reporting.html) resource page.
