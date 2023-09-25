---
layout: workshop-lesson
---

# Figshare API Workshop - Figshare Elements User Day

Here are instructions on how to use your institution's stage repository and the workshop [sandbox repository](./sandbox-instructions.html).


# The Figshare API
Figshare actually has 4 different APIs:
- REST API: https://api.figshare.com/v2
- Stats API: https://stats.figshare.com/ 
- OAI-PMH: https://api.figshare.com/v2/oai 
- ResourceSync: https://scholardata.sun.ac.za/.well-known/resourcesync

# Using the API without scripting

## How to use the documentation

There are three panes on the documentation site
1. Left side: Table of Contents with links to the endpoints and documentation sections
2. Middle: Endpoint details including the endpoint syntax, a short description, error information, and some endpoints can be called directly in this pane
3. Right side: Input and Output examples.
 - If the endpoint accepts data you will see a "Body Sample" and a "Body Schema"
 - If the endpoint provides data you will see a "Response Sample" and a "Response Schema"

![image of figshare api documentation with annotations for the three sections](../assets/api-docs.jpg)

> ### Try it out - retrieve metadata
> You can retrieve all the metadata for this item: "Using APIs to customise repositories and engage audiences" available here: [https://doi.org/10.6084/m9.figshare.5616445.v2](https://doi.org/10.6084/m9.figshare.5616445.v2). Notice the DOI takes you to this URL:
https://figshare.com/articles/presentation/Using_APIs_to_customise_repositories_and_engage_audiences/5616445

> While you can download the citation metadata directly from the user interface, to download all of the metadata you need to use the API. Visit this URL: [https://docs.figshare.com/#article_details](https://docs.figshare.com/#article_details). In the article_id field enter the number that is at the very end of the URL above: 5616445. Then click the red 'TRY' button. A pop up should appear with all the metadata.

> You can view this same output in its own browser tab by visiting the API endpoint with the item id appended to it: [https://api.figshare.com/v2/articles/5616445](https://api.figshare.com/v2/articles/5616445)

> ![image of figshare api documentation showing how to retrieve all the metadata for item 5616445](../assets/api-article-details.jpg)

> ### Try it out - retrieve views

> To retrieve views, you will use a <a href="https://docs.figshare.com/#stats" target="_blank">stats endpoint</a>. The documentation site does not provide an interface to add parameters, but you can construct the URL yourself. Try retrieving the views for the item used above by pasting this URL into a browser tab:

> ```
https://stats.figshare.com/total/views/article/5616445
> ```

>### Try it out - perform a metadata search

>Figshare search will search all metadata fields by default. You can limit to date ranges and order the results in several ways. You can also search within specific metadata fields. In this example, we will search for records that contain the term "frog" in the title and will return 5 results in descending order by published date. Enter the following JSON into the parameters 'search' box at this endpoint: [https://docs.figshare.com/#articles_search](https://docs.figshare.com/#articles_search)

>```
{
  "order": "published_date",
  "search_for": ":title: frog",
  "page": 1,
  "page_size": 5,
  "order_direction": "desc"
}
>```

>![image of figshare api documentation showing how to search for records](../assets/api-search.jpg)

You can retrieve information in this way for items, Projects, and Collections.


## API authentication
- REST API - account token: https://help.figshare.com/article/how-to-get-a-personal-token
- Stats API - Views/downloads/shares for items/collections/projects no authentication needed. Within institution scope, separate username/password required, contact support@figshare.com to get credentials for your institution.

Credentials are sent to the API server via the Authorization header. **Make sure credentials are stored in a secure location!**


> ### Try it out - authenticate and send metadata to create a new item
> 
> 1. Create a token for your personal account or the sandbox account ([sandbox instructions](./sandbox-instructions.html)).
> 2. Navigate to the documentation site (here for personal accounts or here for sandbox accounts) and paste your token in the upper left field.
> 3. On the left panel, click 'Articles' and then click 'Private article', then 'Create new article'. This endpoint accepts JSON formatted metadata and creates a draft article in the account linked to the token you used.
> 4. Copy the JSON below and paste it into the Parameters box (replace 'YOUR NAME' with your name). Click the red 'Try' button.
> 5. Look in your account to see the item. You can delete the draft as well.
> ```
{
  "title": "YOUR NAME made this using the API"
}
> ```

> ![image of figshare api documentation showing how to enter a token and send metadata](../assets/docs-token.jpg)