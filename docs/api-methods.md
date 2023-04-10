---
layout: lesson
---

## Summary of GET, POST, PUT, DELETE methods 

## Example code

### Return results (GET)

Any public metadata or files can be retrieved through the API without authentication. In the example below, the full metadata record for an item is retrieved. The ITEM_ID is the number at the end of any item's URL.

Python:
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
print(json.dumps(metadata, indent=2))
```
Output:
```
{
  "files": [
    {
      "id": 3074543,
      "name": "2011-02-15_14-24-56-228.png",
      "size": 470683,
      "is_link_only": false,
      "download_url": "https://ndownloader.figshare.com/files/3074543",
      "supplied_md5": "eb5c9c6b278b533f98aa02feae57c266",
      "computed_md5": "eb5c9c6b278b533f98aa02feae57c266"
    }
  ],
  "custom_fields": [],
  "authors": [
    {
      "id": 155524,
      "full_name": "Yunyan Deng",
      "is_active": false,
      "url_name": "_",
      "orcid_id": ""
    },
    {
      "id": 155526,
      "full_name": "Jianting Yao",
      "is_active": false,
      "url_name": "_",
      "orcid_id": ""
    },
    {
      "id": 155529,
      "full_name": "Xiuliang Wang",
      "is_active": false,
      "url_name": "_",
      "orcid_id": ""
    },
    {
      "id": 110581,
      "full_name": "Hui Guo",
      "is_active": false,
      "url_name": "_",
      "orcid_id": ""
    },
    {
      "id": 155532,
      "full_name": "Delin Duan",
      "is_active": false,
      "url_name": "_",
      "orcid_id": ""
    }
  ],
  "figshare_url": "https://plos.figshare.com/articles/dataset/Transcriptome_Sequencing_and_Comparative_Analysis_of_Saccharina_japonica_Laminariales_Phaeophyceae_under_Blue_Light_Induction/123456",
  "description": "<div><h3>Background</h3><p>Light has significant effect on the growth and development of <em>Saccharina japonica</em>, but there are limited reports on blue light mediated physiological responses and molecular mechanism. In this study, high-throughput paired-end RNA-sequencing (RNA-Seq) technology was applied to transcriptomes of <em>S. japonica</em> exposed to blue light and darkness, respectively. Comparative analysis of gene expression was designed to correlate the effect of blue light and physiological mechanisms on the molecular level.</p> <h3>Principal Findings</h3><p>RNA-seq analysis yielded 70,497 non-redundant unigenes with an average length of 538 bp. 28,358 (40.2%) functional transcripts encoding regions were identified. Annotation through Swissprot, Nr, GO, KEGG, and COG databases showed 25,924 unigenes compared well (E-value <10<sup>\u22125</sup>) with known gene sequences, and 43 unigenes were putative BL photoreceptor. 10,440 unigenes were classified into Gene Ontology, and 8,476 unigenes were involved in 114 known pathways. Based on RPKM values, 11,660 (16.5%) differentially expressed unigenes were detected between blue light and dark exposed treatments, including 7,808 upregulated and 3,852 downregulated unigenes, suggesting <em>S. japonica</em> had undergone extensive transcriptome re-orchestration during BL exposure. The BL-specific responsive genes were indentified to function in processes of circadian rhythm, flavonoid biosynthesis, photoreactivation and photomorphogenesis.</p> <h3>Significance</h3><p>Transcriptome profiling of <em>S. japonica</em> provides clues to potential genes identification and future functional genomics study. The global survey of expression changes under blue light will enhance our understanding of molecular mechanisms underlying blue light induced responses in lower plants as well as facilitate future blue light photoreceptor identification and specific responsive pathways analysis.</p> </div>",
  "funding": null,
  "funding_list": [],
  "version": 1,
  "status": "public",
  "size": 470683,
  "created_date": "2012-06-27T00:57:36Z",
  "modified_date": "2016-01-19T09:09:40Z",
  "is_public": true,
  "is_confidential": false,
  "is_metadata_record": false,
  "confidential_reason": "",
  "metadata_reason": "",
  "license": {
    "value": 1,
    "name": "CC BY 4.0",
    "url": "https://creativecommons.org/licenses/by/4.0/"
  },
  "tags": [
    "transcriptome",
    "sequencing",
    "comparative",
    "induction"
  ],
  "categories": [
    {
      "id": 69,
      "title": "Inorganic Chemistry",
      "parent_id": 38,
      "path": "",
      "source_id": "",
      "taxonomy_id": 10
    },
    {
      "id": 13,
      "title": "Genetics",
      "parent_id": 48,
      "path": "",
      "source_id": "",
      "taxonomy_id": 10
    },
    {
      "id": 61,
      "title": "Developmental Biology",
      "parent_id": 48,
      "path": "",
      "source_id": "",
      "taxonomy_id": 10
    }
  ],
  "references": [],
  "has_linked_file": false,
  "citation": "Deng, Yunyan; Yao, Jianting; Wang, Xiuliang; Guo, Hui; Duan, Delin (2016): Transcriptome Sequencing and Comparative Analysis of <em>Saccharina japonica</em> (Laminariales, Phaeophyceae) under Blue Light Induction. PLOS ONE. Dataset. https://doi.org/10.1371/journal.pone.0039704",
  "is_embargoed": false,
  "embargo_date": null,
  "embargo_type": null,
  "embargo_title": "",
  "embargo_reason": "",
  "embargo_options": [],
  "id": 123456,
  "title": "Transcriptome Sequencing and Comparative Analysis of <em>Saccharina japonica</em> (Laminariales, Phaeophyceae) under Blue Light Induction",
  "doi": "10.1371/journal.pone.0039704",
  "handle": "",
  "url": "https://api.figshare.com/v2/articles/123456",
  "published_date": "2012-06-27T00:57:36Z",
  "thumb": "https://s3-eu-west-1.amazonaws.com/ppreviews-plos-725668748/3074543/thumb.png",
  "defined_type": 3,
  "defined_type_name": "dataset",
  "group_id": 107,
  "url_private_api": "https://api.figshare.com/v2/account/articles/123456",
  "url_public_api": "https://api.figshare.com/v2/articles/123456",
  "url_private_html": "https://figshare.com/account/articles/123456",
  "url_public_html": "https://plos.figshare.com/articles/dataset/Transcriptome_Sequencing_and_Comparative_Analysis_of_Saccharina_japonica_Laminariales_Phaeophyceae_under_Blue_Light_Induction/123456",
  "timeline": {
    "posted": "2012-06-27T00:57:36",
    "firstOnline": "2016-01-19T09:09:40"
  },
  "resource_title": "Transcriptome Sequencing and Comparative Analysis of <em>Saccharina japonica</em> (Laminariales, Phaeophyceae) under Blue Light Induction",
  "resource_doi": "10.1371/journal.pone.0039704"
}
```

### Authenticate (GET, POST, PUT, DELETE)

Authentication is required for any endpoint that retrieves or accepts private or institutional information. A token can be [created for any user account](https://help.figshare.com/article/how-to-get-a-personal-token) and provides access in line with the account's privileges. In the example below, a user retrieves 10 basic metadata records from their personal account. These records may include both public and private (draft) records. Note that the results are limited to 10 by using the page and page_size parameters. 

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

Sending information through a POST or PUT endpoint is accomplished by adding a 'data' variable to the request. The contents of the data variable needs to be formatted as indicated by the documentation for the API endpoint. 

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

Impersonation is acheived by adding "impersonate":*account id to impersonate* to the parameter content. You must authenticate with a token created from an administrator account for this to work. In the example below, a new record is created in a user account by adding "impersonate" = account_id to the metadata sent to create the new record.

```py
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header -this must be from an administrator account for impersonation
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Get the author info from this endpoint: https://docs.figshare.com/#private_institution_accounts_list
account_id = ENTER ACCOUNT ID #This is 'id' in the endpoint output and is the account id for impersonation
#Create json formatted for upload
sample_metadata = {"title":"Test metadata for upload","keywords":["biodiversity","invertebrate"]}
json_metadata = json.dumps(sample_metadata)
json_metadata["impersonate"] = account_id #Adding this to the metadata for upload enables impersonation
#Create a private item in the user account
r = requests.post(BASE_URL + '/account/articles', headers=api_call_headers, data = json_metadata)
if r.status_code == 201: #If Post was successful
  print('Successfully created item')
```

