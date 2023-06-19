---
layout: workshop-lesson
---

# Creating a custom Web App using the Figshare API

This was demonstrated during the workshop. ChatGPT was used to show how a non-expert can build their own custom web app. Both the workshop example and a complete example are available and require Python installed on your machine. Run the Python script and point your browser to [http://localhost:5000](http://localhost:5000).
- Workshop demonstration files that set up a Python server and a basic HTML page to show titles from a Figshare sandbox environment:
    - [Workshop chatGPT prompts and responses](../assets/chatGPT-web-app-workshop-example.pdf)
	- [Wokshop example zipped file with Python code and HTML page](../assets/workshop.zip)(downloads a zip file)
- Similar to above, but this complete example shows titles and views for records from the sandbox:
    - [Complete chatGPT prompts and responses](../assets/chatGPT-web-app-complete-example.pdf)
	- [Complete example zipped file with Python code and HTML page](../assets/complete.zip)(downloads a zip file)

## Development environment and tools

### Visual Studio
- <a href="https://code.visualstudio.com/" target="_blank">https://code.visualstudio.com/</a>
- <a href="https://code.visualstudio.com/docs/python/python-tutorial" target="_blank">https://code.visualstudio.com/docs/python/python-tutorial</a>
- <a href="https://code.visualstudio.com/docs/python/environments" target="_blank">https://code.visualstudio.com/docs/python/environments </a>

### Server
- Python: <a href="https://www.python.org/" target="_blank">https://www.python.org/</a>
- Flask: <a href="https://flask.palletsprojects.com/en/2.3.x/" target="_blank">https://flask.palletsprojects.com/en/2.3.x/</a>

### Web interface
- React: <a href="https://react.dev/" target="_blank">https://react.dev/</a>

### Code generation
- ChatGPT: <a href="https://chat.openai.com/" target="_blank">https://chat.openai.com/</a>


## Generate code with ChatGPT:
See the pdf examples above for how to refine the prompts.

- Generate Python code for a web server that serves a static file
- Generate a HTML file with a React app that displays the results of a HTTP call
- Generate Python code for a Flask GET route that has a query parameter and returns JSON

## Other useful documentation:
- <a href="https://developer.mozilla.org/en-US/" target="_blank">https://developer.mozilla.org/en-US/</a>

## Security considerations:
- CORS: Abbreviation for cross-origin resource sharing
    - Mechanism which indicates from what origins (http://localhost:5000 in our case) a browser is allowed to make HTTP API requests
    - Security feature which prevents malicious sites stealing user information from legitimate sites
    - By default Figshare’s API does not allow cross-origin requests
	- Solutions:
		- Make API requests via a (proxy) server


