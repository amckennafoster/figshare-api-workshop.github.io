---
layout: workshop-lesson
---

# Getting started with Postman

First set up an environment

![postman example image - set up environment](../assets/postman-create-environment.jpg)

Then add a request

![postman example image - add request](../assets/postman-create-request.jpg)

You can add a token under 'Authorization'

![postman example image - add token](../assets/postman-add-token.jpg)

Even better, save your token as a variable...

![postman example image - save token as variable](../assets/postman-create-variable-for-token.jpg)

Then use that token under Authorization by putting double curly brackets around the variable name ({{variable name}}).

![postman example image - set up environment](../assets/postman-add-token-as-variable.jpg)

Change the request from Get to POST or PUT to match what the API endpoint. You can add information to the body area. This example uses POST with this endpoint: https://docs.figsh.com/#private_article_create
The body contains JSON formatted metadata and the endpoint returns the item id as the 'entity_id' and the api URL for the item as 'location'

![postman example image - POST metadata to create item(../assets/postman-POST-send-body.jpg)