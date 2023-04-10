---
layout: lesson
---

# Sending and Retrieving Information

This page has example code snippets demonstrating how to send and retrieve information through the Figshare API. The [documentation site](https://docs.figshare.com) has more details on all endpoints and includes information on GET, POST, PUT, and DELETE methods.

- [Return results](#return-results)
- [Authenticate](#authenticate)
- [Send data](#send-data)
- [Impersonate](#impeersonate)


### [Return results] (GET)

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


### [Authenticate] (GET, POST, PUT, DELETE)

Authentication is required for any endpoint that retrieves or accepts private or institutional information. A token can be [created for any user account](https://help.figshare.com/article/how-to-get-a-personal-token) and provides access in line with the account's privileges. In the example below, a user retrieves 10 basic metadata records from their personal account. These records may include both public and private (draft) records. Note that the results are limited to 10 by using the page and page_size parameters. 

Python:
```py
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Retrieve basic metadata for 10 items your account owns
r=requests.get(BASE_URL + '/account/articles?page=1&page_size=10', headers=api_call_headers)
#Load the metadata as JSON
metadata=json.loads(r.text)
#View the metadata
print(metadata)
```

Output:
```
[
  {
    "id": 8365555,
    "title": "Testing-full embargo-restrict access-unpublish-republish",
    "doi": "10.0166/FK2.stagefigshareare.8365555",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8365555",
    "published_date": "2023-04-06T13:13:41Z",
    "thumb": "",
    "defined_type": 3,
    "defined_type_name": "dataset",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8365555",
    "url_public_api": "https://api.figshare.com/v2/articles/8365555",
    "url_private_html": "https://figshare.com/account/articles/8365555",
    "url_public_html": "https://faber.figshare.com/articles/dataset/Testing-full_embargo-restrict_access-unpublish-republish/8365555",
    "timeline": {
      "posted": "2023-04-06T13:13:41",
      "firstOnline": "2023-04-06T13:09:40"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8356369,
    "title": "My example dataset",
    "doi": "10.0166/FK2.stagefigshareare.8356369",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8356369",
    "published_date": "2023-04-04T21:36:57Z",
    "thumb": "",
    "defined_type": 3,
    "defined_type_name": "dataset",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8356369",
    "url_public_api": "https://api.figshare.com/v2/articles/8356369",
    "url_private_html": "https://figshare.com/account/articles/8356369",
    "url_public_html": "https://faber.figshare.com/articles/dataset/My_example_embargoed_record_-_institutional_repository_version/8356369",
    "timeline": {
      "posted": "2023-04-04T21:36:57",
      "firstOnline": "2023-04-04T21:36:01"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8351662,
    "title": "Untitled Item",
    "doi": "",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8351662",
    "published_date": null,
    "thumb": "",
    "defined_type": 0,
    "defined_type_name": "",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8351662",
    "url_public_api": "https://api.figshare.com/v2/articles/8351662",
    "url_private_html": "https://figshare.com/account/articles/8351662",
    "url_public_html": "https://faber.figshare.com/articles/dataset/_/8351662",
    "timeline": {},
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8332638,
    "title": "Ant behaviour data set",
    "doi": "10.0166/FK2.stagefigshareare.8332638",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8332638",
    "published_date": "2023-03-24T15:16:19Z",
    "thumb": "https://s3-eu-west-1.amazonaws.com/testfigsharearepreviews.figshareare.com/830360616/thumb.png",
    "defined_type": 3,
    "defined_type_name": "dataset",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8332638",
    "url_public_api": "https://api.figshare.com/v2/articles/8332638",
    "url_private_html": "https://figshare.com/account/articles/8332638",
    "url_public_html": "https://faber.figshare.com/articles/dataset/Ant_behaviour_data_set/8332638",
    "timeline": {
      "posted": "2023-03-24T15:16:19",
      "firstOnline": "2023-03-24T15:16:19"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8329418,
    "title": "A versioned item - unpublished - republished",
    "doi": "10.0166/FK2.stagefigshareare.8329418",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8329418",
    "published_date": "2023-04-06T20:35:25Z",
    "thumb": "https://s3-eu-west-1.amazonaws.com/testfigsharearepreviews.figshareare.com/830356944/thumb.png",
    "defined_type": 2,
    "defined_type_name": "media",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8329418",
    "url_public_api": "https://api.figshare.com/v2/articles/8329418",
    "url_private_html": "https://figshare.com/account/articles/8329418",
    "url_public_html": "https://faber.figshare.com/articles/media/A_versioned_item_-_unpublished_-_republished/8329418",
    "timeline": {
      "posted": "2023-04-06T20:35:25",
      "firstOnline": "2023-03-23T18:42:20"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8327074,
    "title": "Data supporting wood ants behaviour",
    "doi": "10.0166/FK2.stagefigshareare.8327074",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8327074",
    "published_date": "2023-03-22T09:57:21Z",
    "thumb": "https://s3-eu-west-1.amazonaws.com/testfigsharearepreviews.figshareare.com/830355168/thumb.png",
    "defined_type": 3,
    "defined_type_name": "dataset",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8327074",
    "url_public_api": "https://api.figshare.com/v2/articles/8327074",
    "url_private_html": "https://figshare.com/account/articles/8327074",
    "url_public_html": "https://faber.figshare.com/articles/dataset/Data_supporting_wood_ants_behaviour/8327074",
    "timeline": {
      "posted": "2023-03-22T09:57:21",
      "firstOnline": "2023-03-22T09:57:21"
    },
    "resource_title": "A motion compensation treadmill for untethered wood ants (Formica rufa): evidence for transfer of orientation memories from free-walking training",
    "resource_doi": "10.1242/jeb.228601"
  },
  {
    "id": 8275510,
    "title": "DRI Sample Report.pdf",
    "doi": "10.0166/FK2.stagefigshareare.8275510",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8275510",
    "published_date": "2023-04-07T16:12:14Z",
    "thumb": "",
    "defined_type": 3,
    "defined_type_name": "dataset",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8275510",
    "url_public_api": "https://api.figshare.com/v2/articles/8275510",
    "url_private_html": "https://figshare.com/account/articles/8275510",
    "url_public_html": "https://faber.figshare.com/articles/dataset/DRI_Sample_Report_pdf/8275510",
    "timeline": {
      "posted": "2023-04-07T16:12:14",
      "firstOnline": "2023-04-07T16:12:14"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8271566,
    "title": "Exhibition B Image",
    "doi": "10.0166/FK2.stagefigshareare.8271566",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8271566",
    "published_date": "2023-02-20T15:09:10Z",
    "thumb": "https://s3-eu-west-1.amazonaws.com/testfigsharearepreviews.figshareare.com/830323572/thumb.png",
    "defined_type": 2,
    "defined_type_name": "media",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8271566",
    "url_public_api": "https://api.figshare.com/v2/articles/8271566",
    "url_private_html": "https://figshare.com/account/articles/8271566",
    "url_public_html": "https://faber.figshare.com/articles/media/Exhibition_B_Image/8271566",
    "timeline": {
      "posted": "2023-02-20T15:09:10",
      "firstOnline": "2023-02-20T15:09:10"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8262674,
    "title": "Ireland pub C18th",
    "doi": "10.0166/FK2.stagefigshareare.8262674",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8262674",
    "published_date": "2023-02-16T11:29:15Z",
    "thumb": "https://s3-eu-west-1.amazonaws.com/testfigsharearepreviews.figshareare.com/830318778/thumb.png",
    "defined_type": 1,
    "defined_type_name": "figure",
    "group_id": 9991,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8262674",
    "url_public_api": "https://api.figshare.com/v2/articles/8262674",
    "url_private_html": "https://figshare.com/account/articles/8262674",
    "url_public_html": "https://faber.figshare.com/articles/figure/Ireland_pub_C18th/8262674",
    "timeline": {
      "posted": "2023-02-16T11:29:15",
      "firstOnline": "2023-02-16T11:29:15"
    },
    "resource_title": null,
    "resource_doi": null
  },
  {
    "id": 8260098,
    "title": "Slides",
    "doi": "10.0166/FK2.stagefigshareare.8260098",
    "handle": "",
    "url": "https://api.figshare.com/v2/account/articles/8260098",
    "published_date": null,
    "thumb": "https://ndownloader.figshare.com/files/830317022/preview/830317022/thumb.png",
    "defined_type": 7,
    "defined_type_name": "presentation",
    "group_id": 9999,
    "url_private_api": "https://api.figshare.com/v2/account/articles/8260098",
    "url_public_api": "https://api.figshare.com/v2/articles/8260098",
    "url_private_html": "https://figshare.com/account/articles/8260098",
    "url_public_html": "https://faber.figshare.com/articles/presentation/Slides/8260098",
    "timeline": {},
    "resource_title": null,
    "resource_doi": null
  }
]
```

### [Send data] (POST, PUT)

Sending information through a POST or PUT endpoint is accomplished by adding a 'data' variable to the request. The contents of the data variable needs to be formatted as indicated by the documentation for the API endpoint. In the example below, a new record is added to the account that created the token

Python:
```py
import json
import requests
#Set the base URL and item id
BASE_URL = 'https://api.figshare.com/v2'
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

### [Impersonate] (GET, POST, PUT, DELETE)

Impersonation is acheived by adding "impersonate":*account id to impersonate* to the parameter content. You must authenticate with a token created from an administrator account for this to work. In the example below, an existing record is updated (using PUT) in a user account by adding "impersonate" = account_id to updated metadata values. *Note: for these metadata updates to be public, a second API endpoint should be used to publish the record. Or it can be done through the user interface.

```py
import json
import requests
#Set the base URL
BASE_URL = 'https://api.figshare.com/v2'
#Set the token in the header -this must be from an administrator account for impersonation
api_call_headers = {'Authorization': 'token ENTER-TOKEN'} #example: {'Authorization': 'token dkd8rskjdkfiwi49hgkw...'}
#Get the author info from this endpoint: https://docs.figshare.com/#private_institution_accounts_list
account_id = ENTER ACCOUNT ID #This is 'id' in the endpoint output and is the account id for impersonation
#Set the item id for the record to be updated
ITEM_ID = 123456
#Create json formatted for upload
sample_metadata = {"title":"Test metadata for upload","keywords":["biodiversity","invertebrate"]}
json_metadata = json.dumps(sample_metadata)
json_metadata["impersonate"] = account_id #Adding this to the metadata for upload enables impersonation
#Update the private version of the item in the user account
r = requests.put(BASE_URL + '/account/articles/' + str(ITEM_ID), headers=api_call_headers, data = json_metadata)
if r.status_code == 201: #If Put was successful
  print('Successfully updated item')
```

