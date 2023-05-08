---
layout: workshop-lesson
---

# Programmatically Retrieve Metadata

Retreiving metadata can be useful for reporting, error checking, duplicating records, and converting to other metadata schemas. Please note that you can download a subset of the metadata through the [Administrative Batch Management](https://help.figshare.com/article/administrative-batch-management) tool.

Any API endpoint that returns a set of items, like search endpoints and personal account endpoints, will return a subset of the available metadata. These metadata include the item/Collection/Project id. You can use that id to retrieve all the metadata and statistics for that object. 

We'll now run through the examples in the <a href="https://amckennafoster.github.io/samplefigshareapi.github.io/workshop/chain-api-calls.html" target="_blank">how to chain API calls</a> section in the resources part of this site.