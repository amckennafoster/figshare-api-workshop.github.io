---
layout: resource
---

# Formatting JSON

## Formatting json to send
Format authors as part of creating a new item. 
```py
import json
#assume you have a list of json formatted records which includes the first and last names labeled as 'first_name' and 'last_name'.
authors = []
for item in json_metadata:
	for name in item['authors']:
		authorname = {"name" : name['first_name'] + " " + name['last_name']}
		authors.append(authorname)

```

Format a partial date for use in Figshare (partial date support is on the roadmap)
```py

#assume you have created a dictionary called my_dict that holds the Figshare formatted metadata and the harvested metadata only includes a year. This makes the date January 1 of that year.

my_dict['timeline'] =  {"firstOnline" : str(item['year']) + "-01-01"} #year only
```

Here is an example of formatting existing json metadata for upload to Figshare:
```py
existing_metadata = [
    {"record_id":123,
     "title":"blah blah blah",
     "authors": [{"first_name":"Allisa", "last_name":"Morris"},{"first_name":"Alex", "last_name":"Dannold"}],
     "doi":"https://doi.org/10.38743/384738"
    },
    {"record_id":456,
     "title":"yadda yadda",
     "authors": [{"first_name":"Andrew", "last_name":"Yol"},{"first_name":"Rebecca", "last_name":"Jane"}],
     "doi":"https://doi.org/10.29033/6575"}
]

formatted_metadata = []
for record in existing_metadata:
    record_dict = {}
    record_dict['id'] = record['record_id']
    record_dict['title'] = record['title']
    #Format authors
    authors = []
    for name in record['authors']:
        authorname = {"name" : name['first_name'] + " " + name['last_name']}
        authors.append(authorname)
    record_dict['authors'] = authors
    #Format doi to just the doi vaalue and not URL
    doi = str(record['doi'])[16:]
    record_dict['doi'] = doi
    #Add record to the growing list
    formatted_metadata.append(record_dict)
    
#See formatted metadata:
formatted_metadata
```
Output:
```json
[{'id': 123,
  'title': 'blah blah blah',
  'authors': [{'name': 'Allisa Morris'}, {'name': 'Alex Dannold'}],
  'doi': '10.38743/384738'},
 {'id': 456,
  'title': 'yadda yadda',
  'authors': [{'name': 'Andrew Yol'}, {'name': 'Rebecca Jane'}],
  'doi': '10.29033/6575'}]
```

## Formatting downloaded json

If you downloaded metadata and want to format the dates in a dataframe:
```py

import pandas as pd

#This assumes you have a dictionary called item_metadata and a dataframe of that metadata called df.

#The dates are all contained within one column called 'timeline'. Flatten that column and associate the values with the proper article id in a new dataframe

temp_date_list = []

for item in item_metadata:
    dateitem = item['timeline']
    dateitem['id'] = item['id']
    temp_date_list.append(dateitem)

df_dates = pd.json_normalize(
    temp_date_list 
)

#Replace blank cells with 'null'
df_dates.replace(df_dates.replace(r'^\s*$', 'null', regex=True, inplace = True)) 

#Merge the dataframes
df_formatted = df.merge(df_dates, how='outer', on='id')

print("Dates split out and merged")
```