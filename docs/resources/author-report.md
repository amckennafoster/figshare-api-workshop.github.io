---
layout: resource
---

# Create an author report

The API can provide information for individual authors. As an example, we created a <a href="https://colab.research.google.com/drive/1tl25Lc_SGk1OKjnMA54HeRaVM3VYUpYP?usp=sharing" target="_blank">Jupyter Notebook in Google Colab</a>. 

Anyone can run this script, though you'll need to sign into a Google account to do so. Then you must only add an author name or an ORCID. The script produces a table with metadata for the author's outputs, views and downloads, and produces a plotly map of views for all the records.

Here is the order of operations:
1. Import libraries and set base API URL
2. Perform search for name or ORCID using this endpoint: https://docs.figshare.com/#articles_search  Then create a dataframe
3. Gather views and downloads for each record from this endpoint: https://docs.figshare.com/#stats_totals Then create a dataframe
4. Merge the metadata and stats dataframes
5. Optionally collect all the same information for Collections using the collection endpoints
6. Format the date information (extract dates from the JSON and add them to the dataframe
7. Gather views by country for each item using this endpoint: https://docs.figshare.com/#stats_breakdown
8. Map the views
9. Save the dataframe and you can copy the map images from the browser