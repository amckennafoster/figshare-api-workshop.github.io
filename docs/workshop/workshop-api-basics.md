---
layout: workshop-lesson
---

# The Figshare API
Figshare actually has 4 different APIs:
- REST API: https://api.figshare.com/v2
- Stats API: https://stats.figshare.com/ 
- OAI-PMH: https://api.figshare.com/v2/oai 
- ResourceSync: https://scholardata.sun.ac.za/.well-known/resourcesync

### Try it out
You can retrieve all the metadata for this item: "Using APIs to customise repositories and engage audiences" available here: [https://doi.org/10.6084/m9.figshare.5616445.v2](https://doi.org/10.6084/m9.figshare.5616445.v2). Notice the DOI takes you to this URL:
https://figshare.com/articles/presentation/Using_APIs_to_customise_repositories_and_engage_audiences/5616445

While you can download the citation metadata directly from the user interface, to download all of the metadata you need to use the API. Visit this URL: [https://docs.figshare.com/#article_details](https://docs.figshare.com/#article_details). In the article_id field enter the number that is at the very end of the URL above: 5616445. Then click the red 'TRY' button. A pop up should appear with all the metadata.

You can view this same output in its own browser tab by visiting the API endpoint with the item id appended to it: [https://api.figshare.com/v2/articles/5616445](https://api.figshare.com/v2/articles/5616445)

![image of figshare api documentation showing how to retrieve all the metadata for item 5616445](../assets/api-article-details.jpg)

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
- Stats API - user/password, contact support@figshare.com for an account

Credentials are sent to the API server via the Authorization header. **Make sure credentials are stored in a secure location!**

