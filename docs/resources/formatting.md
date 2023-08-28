---
layout: resource
---

# Formatting JSON

## Formatting json to send
### Authors
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
### Dates
Format a partial date for use in Figshare (partial date support is on the roadmap)
```py

#assume you have created a dictionary called my_dict that holds the Figshare formatted metadata and the harvested metadata only includes a year. This makes the date January 1 of that year.

my_dict['timeline'] =  {"firstOnline" : str(item['year']) + "-01-01"} #year only
```
### Format existing JSON
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
```
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

### Dates
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

#Merge the dataframes
df_formatted = df.merge(df_dates, how='outer', on='id')

print("Dates split out and merged")
```
### Group information
In the downloaded metadata, the group is identified with its id. If you are a repository administrator use this script to retrieve the group names and merge them with your dataframe of metadata.
In this example, the df_formatted dataframe is the downloaded metadata and must have a group_id column. Groups can be nested so this script will add the group name matching the group id in the downloaded metadata as well as the parent group.

```py

import pandas as pd

#Get list of groups.
s=requests.get(BASE_URL + '/account/institution/groups', headers=api_call_headers)
groups=json.loads(s.text)

#Create a dataframe of groups
df_groups = pd.json_normalize(groups)

df_groups_parent = df_groups[['id','name']] #Create reference dataframe
df_groups = df_groups.rename(columns={'id': 'group_id','name': 'group_name'}) #Rename id col in main dataframe
df_groups_parent = df_groups_parent.rename(columns={'name': 'parent_group_name'}) #Rename name col in reference dataframe

df_groups = df_groups.sort_values(by=['parent_id'])
top_group_id = df_groups.iloc[0]['group_id'] #Store the group id for top group

df_groups.loc[df_groups['parent_id'] == 0, 'parent_id'] = top_group_id #For top level group, replace the zero value parent id with top level group id

df_groups = df_groups.merge(df_groups_parent, how='inner',left_on=['parent_id'], right_on=['id']) #Add parent group name

df_groups = df_groups[['group_id','group_name','parent_group_name']] #Pare down to needed columns


#Merge the dataframes
df_formatted = df_formatted.merge(df_groups, how='inner', on='group_id') #If you use 'outer' it will include a blank record for each group with no records

print("Names for",len(df_groups),"different groups were added to the metadata records")
```
### Custom fields
The custom field information is stored as a JSON dictionary. The following script converts the JSON to columns in a dataframe which is then merged back into the main metadata dataframe.
The metadata dataframe, df_formatted, needs to have the item id column named 'id'.

```py
import pandas as pd

#The custom fields are all contained within one column called 'custom_fields'. Flatten that column and associate the values
#with the proper article id in a new dataframe
custom = pd.json_normalize(
    item_metadata,
    record_path =['custom_fields'],
    meta=['id']
)

if len(custom) > 0:
    #This reshapes the data so that metadata field names are columns and each row is an id.
    custom = custom.pivot(index="id", columns="name", values="value")

    #Merge the dataframes so that all the custom fields are visible along with all the other metadata
    df_formatted = df_formatted.merge(custom, how='outer', on='id') #Outer merge keeps records that have no custom metadata.
    print('Custom fields split out and merged')
else:
    print('No custom fields to split out')
```