---
layout: resource
---

# Get metadata for items in review

There are several API endpoints that provide <a href="https://docs.figshare.com/#account_institution_curations" target="_blank">review/curation information</a>. Reviewers can sort and inspect items in review through the GUI. But maybe you'd like to see a spreadsheet of the metadata for items that have a 'pending' status for review. 
This <a href="https://colab.research.google.com/drive/1vATcAQfK2hEuX-UYAutrj5iQnAoL6Hfn?usp=sharing" target="_blank">Google Colab notebook</a> (also available as a <a href="https://github.com/amckennafoster/figshare-api-scripts/blob/main/download-metadata-admin/RETRIEVE-ITEMS-IN-REVIEW.ipynb" target="_blank">Jupyter notebook</a>) does just that. Be sure to use an admin token (not just a reviewer account token).

Here are the steps in that notebook:

1. Retrieve a list of items with pending status in review
2. Visit each item and retrieve all the metadata. Also add the owner name and email.
3. Create a spreadsheet and do some formatting: dates, authors, add item owner, separate out custom fields.
4. Save the spreadsheet

### What if I want to publish those items?
You may want to bulk publish a group of items but avoid having to review them all. This isn't possble through the API since that would defeat the purpose of review. However, you can use the Admin Batch Management tool to bulk publish items with review (making the assumption you reviewed the metadata in the CSV!). To do this you need to create a CSV with the item ids to publish. You can do this for pending review items that are in the spreadsheet from above. Here are the steps:

1. Save a copy of the spreadsheet downloaded above as CSV (*important: if using Excel, save as CSV rather than CSV UTF-8. The latter sometimes adds additional characters in the file*)
2. Delete all the items (rows) that you don't want to publish
3. Delete all columns except 'id'
4. Rename the 'id' column to 'article_id' and save the file
5. You should now have a CSV with only one column of numbers titled 'article_id'
6. Log in to an administrator Figshare account that has review privileges. 
7. Navigate to the [Batch Management](https://help.figshare.com/article/administrative-batch-management) tool and select your CSV as the source file. Select the option to automatically review items.


