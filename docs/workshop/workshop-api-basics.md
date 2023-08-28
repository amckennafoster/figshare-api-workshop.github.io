---
layout: workshop-lesson
---

# The Figshare API
Figshare actually has 4 different APIs:
- REST API: https://api.figshare.com/v2
- Stats API: https://stats.figshare.com/ 
- OAI-PMH: https://api.figshare.com/v2/oai 
- ResourceSync: https://scholardata.sun.ac.za/.well-known/resourcesync

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

## Open API and Swagger
- OpenAPI
    - A specification language for REST APIs
    - Allows prototyping and generating documentation, client code and test cases
- Swagger:
    - OpenAPI implementation
    - Editor: https://editor.swagger.io/
    - Swagger UI: https://swagger.io/tools/swagger-ui/ 
- Figshare and Open API
    - Documentation publicly available at https://docs.figshare.com
    - Source code for documentation: https://github.com/figshare/user_documentation
    - Swagger file: https://github.com/figshare/user_documentation/blob/master/swagger_documentation/swagger.json 

## API authentication
- REST API - personal token: https://help.figshare.com/article/how-to-get-a-personal-token
- OAuth2 - create applications which require users to log into their own Figshare account
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