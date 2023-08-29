---
layout: resource
---

# Reporting Example

## Download repository metadata and statistics for reporting

This is an example of how one might use the API to create a database for reporting purposes. The script downloads metadata from your repository along with views by item, views by country, and views by month. Eight tables are saved as worksheets within a Google Sheet. This sheet can be regularly updated so that any dashboards built off it will show current information.

Note: You need to have two sets of credentials:

1. A repository administrator token
2. The credentials for the stats API (ask Support if you do not have them yet)

The <a href="https://colab.research.google.com/drive/17GtCZHfRT6kDe_Rbrj9EkIpnxyO3iuRH?usp=sharing" target="_blank">Jupyter Notebook</a> is available and you can run it in Colab. Here are the high level steps in the process:
1. Import libraries
2. Set variables and metadata API credentials
3. Retrieve all item ids and pull out only public item ids
4. Retrieve full metadata and total views for each item id
5. Create dataframes for each type of information: items, authors, funding, categories, and tags. Each dataframe includes the item id.
6. Set variables and credentials for the stats API
7. Use the stats API to retrieve views by country and over time.
8. Either create a Google sheet or update an existing one

## Visualize the data

You should be able to use the resulting spreadsheet as a data source for you favorite visualization software. We made a very basic proof of concept <a href="https://lookerstudio.google.com/reporting/21c1ab3b-f1a1-44dd-9bc0-ff8665650a5c" target="_blank">Looker Studio</a> dashboard based on data from a live repository.
