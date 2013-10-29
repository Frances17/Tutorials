### Assumption:
You have a working knowledge of JavaScript and a saved RAML API definition.

### What the Notebook Is:
An interactive JavaScript programming environment that can embed documentation.

### What the Notebook Does:
Generates an API client based on a saved RAML API definition so that authenticated calls can be made to the server, returning real-time information. 

### Why is the Notebook so useful?
RAML translates the idea of a practically-RESTful API into a workable definition. The Notebook takes that API definition and immediately allows the API consumer to begin interacting with it. Use the Notebook as a ready-made, persisting sandbox. Test multiple potential APIs simultaneously with live calls to the client servers, even posting information to make updates in real-time. The Notebook's autocomplete functionality uses the saved RAML API definition to prompt the developer with what is available at that level in the code to prevent flipping back and forth from the documentation. Any changes you make to the Notebook are saved directly into your own Github gists, and you can load another developer's, too. That means you can load someone else's demo and then save your own copy to start working where they left off.

### Notebook Components

The notebook has two types of cells for code and documentation:

**Code cells** are the default type for a newly created cell. They contain executable javascript with support for RAML-based API client generation. To run all code cells at once, use the "play all" button in the top right. To run an individual cell, click to give it focus and hit enter.

**Documentation cells** can also be created if you open and close a cell's contents using java-style block comments. The cell will become a markdown-formatted documentation cell.

### Step 1: Create an API Client:

Create an API client so that the Notebook has something from which to something pull:

```
API.createClient
```

Notice that as you start working, the Notebook prompts you with several autocomplete options. For this example, use the RAML definition hosted at APIhub for Instagram. 

```
API.createClient('instagram', 'http://www.apihub.com/raml-spec/10914');
```

Upon entering this command into the Notebook and hitting enter to "play" the cell, the client-generator will return a number of functions that are now available to you. Review locations, users, and more. You can see that authentication is also available.

### Step 2: Authenticate

Authenticate with OAuth 2.0 sooner rather than later. You'll need to register any new application at <http://instagram.com/developer> with the redirect_uri **{APINotebookHost}/authentication/oauth2.html**. Insert the client ID and secret given to you in a code cell as follows, and run the cell: 

```
instagram.authenticateOauth20 ({clientId: 'clientID', clientSecret: 'secret'});
```

If successful, an auth dialog window will open so you can log in with valid Instagram user credentials. **You may have to enable popups in your browser.**

The Notebook delivers the request to the server for you, and returns your accessToken. The Notebook works sequentially: because you've entered in this request at the beginning, every call you make to the server will now incorporate the required security information.

### Step 3: Making Calls

Now let's make a request to the server. 

What if you wanted to see what's popular on Instagram? Autocompletion takes you most of the way there:

```
instagram.media.popular.get();
```
Not only is the Notebook useful for testing out an API that you are thinking of using, it's also handy for testing an API that you are in the process of writing. The Notebook can be as collaborative as you want - simply share the URL with the gist ID appended: <http://www.apihub.com/raml/api-notebook#0acea96140f97e33b203>. Do the same when you'd like to re-open a Notebook you previously worked on. To enable changes to an existing Notebook: authenticate with Github, then click "make my own copy". You just forked the original notebook, and are working on your own copy. 

