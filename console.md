Console

### Assumption: 
An API definition in RAML already exists. 

### What is the Console?
The Console is the interactive documentation environment for the API consumer: a developer wishing to learn and interact with your API. The console is also useful for the API owner who would like to execute calls against his own API, such as for testing purposes.

### Where can I find the Console?
The Console exists on Github and other open-source respositories, as well as at raml.org. We also encourage you to use the Console for interactive documentation for your targeted API consumers on APIhub. When used in conjunction with the API Designer, users can see the actual RAML definition with which they are interacting, and as the API owner can make changes in real time.

### When do I use the Console?
The Console is useful on a few occasions. While writing the API in the API Designer, watching it translate to the Console is handy as a graphical representation of your API's concepts and characteristics. The Console is also useful as the interactive documentation aspect of your API. API consumers can immediately interact with the server with live test calls, much like in the Notebook. It serves as a gut check for "Can I do this?"

Each cell in the console represents a callable resource. On the right side of each cell, find the methods that are available for that resource. Hovering over the cell expands the icons into DELETE, GET, POST, etc. At this unexpanded level, you can also see if the resource is of a certain type, or uses certain traits or security schemes. 

SCREEN SHOT

Click to expand the cell.

At this point, there are a number of tabs visible to the user. The more methods available for that resource, the more tabs with specific parameters, responses, and description defined for that call. 

SCREEN SHOT

Coming Soon: Testing in the RAML Console.

The console works only with a valid RAML spec to you catch mistakes in your definition as you proceed.