---
layout: resource
---

# Search for records related to your institution

You can search for your institution's name and do a high level analysis of the records out there. As an example, we created a <a href="https://colab.research.google.com/drive/15JGN6dx6OXfqQ2779TCmOhqQEOrLyt5u?usp=sharing" target="_blank">Jupyter Notebook in Google Colab</a>. 

Anyone can run this script. One must only add search criteria and run all the cells in sequence. You can also save all the data as a Google sheet in your Google Drive.

Here is the order of operations:
1. Import libraries and set base API endpoint and institution name
2. Search using this endpoint: https://docs.figshare.com/#articles_search and create a dataframe
3. Collect the full metadata for each item and create separate lists for item metadata, authors, funding, categories, tags, and files.
4. Do some formatting and make dataframes from all the lists
5. Start analysis by looking at the main dataframe columns
6. Look at how many records are from institutions (using the existence of group_id)
7. Item types with views
8. Look at licenses 
9. See a list of linked funders (linked to Dimensions records)
10. See grant titles
11. See the top 20 tags
12. See the top 20 categories
13. See top 10 viewed items
14. Export all the data to a Google sheet that you can then use for visualizations or other analysis
