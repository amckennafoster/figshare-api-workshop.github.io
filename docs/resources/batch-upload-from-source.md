---
layout: resource
---

# Create metadata records from harvested metadata

This pages offers an example of creating lnked file records based on harvested metadata. The resulting items clearly indicate the URL or DOI the user can click to find the original record. The example below is for paper metadata, but it can easily work with data metadata too. Macquarie University created a script to <a href="https://github.com/mq-eresearch/dryad_to_figshare/tree/v1.0.0" target="_blank">create catalog records</a> in their repository from Dryad records.


Here are the steps to format metadata harvested from the Dimensions database and create linked file records:

1. Open a json file
2. Pull out the relevant fields and give them the proper keys (account for partial dates, author formatting,and missing abstracts)
3. Interate through and upload the records 
4. Convert the json record to a string with double quotes
5. Upload the record
6. Update the author list of the new record (removes the admin account as an author)
7. Add the existing DOI as a linked file

This can upload to a specific group with specific custom metadata. You can change the api key to upload to different accounts or use impersonation.

Here is an example Python script. Note that this does not add Categories or Keywords which are required for publishing.

```py
import json
import requests
import pandas as pd

#Set the token in the header and base URL (change the file name and location)

text_file = open("../../../testing-token-symlilliput.txt", "r")
TOKEN = text_file.read()
TOKEN.strip() #removes any hidden spaces
text_file.close()

#Alternatively, you can just enter your token here and comment out the method above.
#TOKEN = str(ENTER TOKEN HERE WITH QUOTES)

api_call_headers = {'Authorization': 'token ' + TOKEN}

#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'

#Upload a json file in case you want to try your own harvested metadata
#with open("NAME.json", "r", encoding='utf8') as read_file: #Replace this with the filename of your choice
#    jsonfile = json.load(read_file)

#For demonstration, create a json file with 2 records- this is json formatted metadata from the Dimensions API
jsonfile = [
	{
		"abstract": "As robots are deployed to work in our environments, we must build appropriate expectations of their behavior so that we can trust them to perform their jobs autonomously as we attend to other tasks. Many types of explanations for robot behavior have been proposed, but they have not been fully analyzed for their impact on aligning expectations of robot paths for navigation. In this work, we evaluate several types of robot navigation explanations to understand their impact on the ability of humans to anticipate a robot’s paths. We performed an experiment in which we gave participants an explanation of a robot path and then measured (i) their ability to predict that path, (ii) their allocation of attention on the robot navigating the path versus their own dot-tracking task, and (iii) their subjective ratings of the robot’s predictability and trustworthiness. Our results show that explanations do significantly affect people’s ability to predict robot paths and that explanations that are concise and do not require readers to perform mental transformations are most effective at reducing attention to the robot.",
		"authors": [
			{
				"affiliations": [
					{
						"city": "Pittsburgh",
						"city_id": 5206379,
						"country": "United States",
						"country_code": "US",
						"id": "grid.147455.6",
						"name": "Carnegie Mellon University",
						"raw_affiliation": "Carnegie Mellon University, Pittsburgh, PA, USA",
						"state": "Pennsylvania",
						"state_code": "US-PA"
					}
				],
				"corresponding": "",
				"current_organization_id": "",
				"first_name": "Stephanie",
				"last_name": "Rosenthal",
				"orcid": [],
				"raw_affiliation": [
					"Carnegie Mellon University, Pittsburgh, PA, USA"
				],
				"researcher_id": "null"
			},
			{
				"affiliations": [
					{
						"city": "Pittsburgh",
						"city_id": 5206379,
						"country": "United States",
						"country_code": "US",
						"id": "grid.147455.6",
						"name": "Carnegie Mellon University",
						"raw_affiliation": "Carnegie Mellon University, Pittsburgh, PA, USA",
						"state": "Pennsylvania",
						"state_code": "US-PA"
					}
				],
				"corresponding": "",
				"current_organization_id": "",
				"first_name": "Peerat",
				"last_name": "Vichivanives",
				"orcid": [],
				"raw_affiliation": [
					"Carnegie Mellon University, Pittsburgh, PA, USA"
				],
				"researcher_id": "null"
			},
			{
				"affiliations": [
					{
						"city": "Pittsburgh",
						"city_id": 5206379,
						"country": "United States",
						"country_code": "US",
						"id": "grid.147455.6",
						"name": "Carnegie Mellon University",
						"raw_affiliation": "Carnegie Mellon University, Pittsburgh, PA, USA",
						"state": "Pennsylvania",
						"state_code": "US-PA"
					}
				],
				"corresponding": "",
				"current_organization_id": "grid.147455.6",
				"first_name": "Elizabeth",
				"last_name": "Carter",
				"orcid": [
					"0000-0002-2735-148X"
				],
				"raw_affiliation": [
					"Carnegie Mellon University, Pittsburgh, PA, USA"
				],
				"researcher_id": "ur.012471523754.61"
			}
		],
		"date": "2022-12-31",
		"doi": "10.1145/3526104",
		"id": "pub.1147121755",
		"title": "The Impact of Route Descriptions on Human Expectations for Robot Navigation",
		"year": 2022
	},
	{
		"abstract": "Data-driven network intrusion detection (NID) has a tendency towards minority attack classes compared to normal traffic. Many datasets are collected in simulated environments rather than real-world networks. These challenges undermine the performance of intrusion detection machine learning models by fitting machine learning models to unrepresentative “sandbox” datasets. This survey presents a taxonomy with eight main challenges and explores common datasets from 1999 to 2020. Trends are analyzed on the challenges in the past decade and future directions are proposed on expanding NID into cloud-based environments, devising scalable models for large network data, and creating labeled datasets collected in real-world networks.",
		"authors": [
			{
				"affiliations": [
					{
						"city": "Pittsburgh",
						"city_id": 5206379,
						"country": "United States",
						"country_code": "US",
						"id": "grid.147455.6",
						"name": "Carnegie Mellon University",
						"raw_affiliation": "Carnegie Mellon University, Pittsburgh, PA",
						"state": "Pennsylvania",
						"state_code": "US-PA"
					}
				],
				"corresponding": "",
				"current_organization_id": "",
				"first_name": "Dylan",
				"last_name": "Chou",
				"orcid": [],
				"raw_affiliation": [
					"Carnegie Mellon University, Pittsburgh, PA"
				],
				"researcher_id": "null"
			},
			{
				"affiliations": [
					{
						"city": "Notre Dame",
						"city_id": 8469294,
						"country": "United States",
						"country_code": "US",
						"id": "grid.131063.6",
						"name": "University of Notre Dame",
						"raw_affiliation": "University of Notre Dame, Notre Dame, Indiana",
						"state": "Indiana",
						"state_code": "US-IN"
					}
				],
				"corresponding": "",
				"current_organization_id": "grid.131063.6",
				"first_name": "Meng",
				"last_name": "Jiang",
				"orcid": [
					"0000-0002-3009-519X"
				],
				"raw_affiliation": [
					"University of Notre Dame, Notre Dame, Indiana"
				],
				"researcher_id": "ur.010456457135.66"
			}
		],
		"date": "2022-12-31",
		"doi": "10.1145/3472753",
		"id": "pub.1141731091",
		"title": "A Survey on Data-driven Network Intrusion Detection",
		"year": 2022
	}
]

#Format records for upload.

result = []
doi_list = []
for item in jsonfile:
    my_dict={}
    my_dict['title']=item.get('title')
    if 'abstract' in item: #abstract isn't always present
        my_dict['description']=item.get('abstract')
    else:
        my_dict['description']="No description available"
    authors = [] #format authors
    for name in item['authors']:
        authorname = {"name" : name['first_name'] + " " + name['last_name']}
        authors.append(authorname)
    my_dict['authors']= authors
    my_dict['defined_type'] = 'journal contribution'
    my_dict['doi']= item.get('doi')
    my_dict['resource_doi']= item.get('doi')
    my_dict['resource_title']=item.get('title')
    #my_dict['references'] = [item.get('URL')]
    my_dict['timeline'] =  {"firstOnline" : str(item['year']) + "-01-01"} #year only 
    #my_dict['is_metadata_record'] = True #Use these if you want a metadata only record
    #my_dict['metadata_reason'] = 'See publisher version'
    result.append(my_dict)
    doi_list.append(item['doi'])


print(len(result),"records are ready for upload.")


#Upload the records

record_fails = []
partial_record_ids = []
created_record_ids = [] #Use this to delete all the draft records if needed 
success_count = 0
count = 0 #This just tracks what index value the loop is on and is used to connect the metadata with the DOI update

for index, item in enumerate(result):
    jsonresult = json.dumps(item) #Takes one record and makes it a json string (double quotes)
    r = requests.post(BASE_URL + '/account/articles', headers=api_call_headers, data = jsonresult)
    if r.status_code != 201:
        record_fails.append(str(index) + ":" + str(r.content[0:75])) #Add failed index to list with partial description
        count += 1
    else:
        count += 1 #increment here otherwise have to do it at each if statement below
        #Remove the admin account as an author by updating the record just created
        #This uses the article url returned by the API response (r)
        
        # Get the location URL and item id
        response_json = json.loads(r.content)
        new_url = response_json['location']
        item_id = response_json['entity_id']
        created_record_ids.append(item_id)
        
        
        #Format and update authors
        authordict = {}
        authordict['authors'] = item['authors']
        authorjson = json.dumps(authordict) #formats everything with double quotes
        s = requests.put(new_url, headers=api_call_headers, data = authorjson) 
        if r.status_code != 201:
            record_fails.append(str(index) + "failed at author update:" + str(r.content[0:75])) #Add failed index to list with partial description         
            partial_record_ids.append(item_id)
        else:
            #Upload a link as a file
            link = '{"link":"https://doi.org/'+ str(doi_list[count-1]) + '"}' #count-1 because already incremented value to next index
            t = requests.post(new_url +'/files', headers=api_call_headers, data = link)
            if r.status_code != 201:
                record_fails.append(str(index) + "failed at doi update:" + str(r.content[0:75])) #Add failed index to list with partial description
                partial_record_ids.append(item_id)
            else:
                success_count += 1

        
print(success_count,"records created and updated. There were",len(result)-len(created_record_ids),"records that were not created at all.")
print('There were',len(partial_record_ids),'records created but with a failed DOI link.')
print("Failed record descriptions:",record_fails)
```
