---
layout: resource
---

# Reporting Example

## Download repository metadata and statistics for reporting

This is an example of how one might use the API to create a database for reporting purposes. The script downloads all the metadata for your repository and also retrieves views by item, views by country, and views over time.

The Jupyter Notebook is available for download. Here are the high level steps in the process:
1. Import libraries
2. Set variables and credentials
3. Retrieve all item ids and pull out only public item ids
4. Retrieve full metadata and total views for each item id
5. Create dataframes for each type of information: items, authors, funding, categories, and tags. Each dataframe includes the item id.
6. Use the stats API to retrieve views by country and over time.
7. Save all dataframes as sheets within one Excel workbook.

## Visualize the data

The Excel file can function like a relational database to some extent because the item id provides a link between most tables. For examples, you can create a table or chart of total views by author by linking the main metadata table and the author table.

We made a very basic proof of concept href="https://lookerstudio.google.com/reporting/21c1ab3b-f1a1-44dd-9bc0-ff8665650a5c" target="_blank">Looker Studio</a> dashboard based on data from a Figshare sandbox.
