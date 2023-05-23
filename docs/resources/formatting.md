---
layout: resource
---

# Formatting JSON

1. Some basic advice (using quotes and json.loads)
2. json validation against the schema??????? https://www.newtonsoft.com/json/help/html/JsonSchema.htm#:~:text=The%20simplest%20way%20to%20check,method%20with%20the%20JSON%20Schema.&text=To%20get%20validation%20error%20messages,%2C%20JsonSchema%2C%20ValidationEventHandler)%20overloads.

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