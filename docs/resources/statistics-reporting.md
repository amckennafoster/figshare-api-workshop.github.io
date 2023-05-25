---
layout: resource
---

# Reporting


The stats endpoints might be used for a variety of reporting purposes:
1. Look up specific statistics for one time reporting
2. Gather statistics for a report produced periodically
3. Harvest item specific statistics for analysis with other metadata
4. And many more

The script for an [author report](./author-report.html) is a good example of a report an individual could run periodically.

## Creating Dashboards

You may want to create your own dashboards or integrate information from your repository into an existing dashboard. Figshare's two user interface dashboards pull data from a database rather than the API and thus provide up to date information rapidly. 

*Performance considerations*

If you want to use the stats API to create a custom dashboard, you'll need to consider performance. For example, downloading statistics for individual items will likely require one API call per item. If you have many items, this would not be feasible.

*Dashboard from downloaded data*

One option is to pre download all the metadata you would like to report on and store the information in a set of tables. A script can be run at regular intervals to update the information and a dashboard can be built using your preferred software. 

See an example of a dashboard created in <a href="https://lookerstudio.google.com/reporting/21c1ab3b-f1a1-44dd-9bc0-ff8665650a5c" target="_blank">Looker Studio</a>. There is a description of how this was created on [this resource page](./example-metadata-download.html).

*Dashboard from the Batch Management tool and Stats API*
Administrators can use the Batch Management tool to download all or some of the metadata from their repository. A script can be run to then gather statistics based on the item ids in the downloaded metadata.
