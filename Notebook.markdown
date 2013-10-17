Notebook

### Assumption:
You have prepared a RAML API definition, although the Notebook will also work with other modelling languages, such as YAML.

### What the Notebook Is:
An interactive JavaScript programming environment with embedded documentation.

### What the Notebook Does:
Generates an API client based on a saved RAML definition so that calls can be made to the server, returning authenticated, real-time information. 

### Why is the Notebook so useful?
RAML's strength lies in its ability to translate the idea of a practically-RESTful API into a workable definition. The Notebook takes the API definition and immediately allows the API consumer to begin interacting with it. Use the Notebook when you'd like to immediately consume or test out an API, at the proof-of-concept. You can actively onboard the API in a live environment with calls to the client server. The autocomplete functionality simplifies the developer's life with prompts for what is available to the developer at that point in the code so that you're not flipping back and forth from the documentation to the Notebook, using the referenced API definition. The Notebook is like a ready-made, persisting sandbox not isolated from the server. If your developer access token gives you a scope allowing you to post information, then you can actively update the server with new information in real-time. Any GET request will return real user information. The Notebook is the perfect playground, allowing you to immediately begin interacting with multiple APIs, seeing how they interact together and inviting collaboration with the ability to fork directly into your own Github repo. 

### Notebook Components

The notebook has two types of cells for code and documentation:

*Code cells* are the default type for a newly created cell. They contain executable javascript with support for RAML-based API client generation. To run all code cells at once, use the "play all" button in the top right. To run an individual cell, click to give it focus and hit enter.

*Documentation cells* can be created if you open and close a cell's contents using java-style block comments. The cell will become a markdown-formatted documentation cell like this one.

Click the ? icon for help at any time, or use shift-? for quick access.

Step 1: Create an API Client:

Create an API client so that the Notebook has something from which to something pull:

```
API.createClient
```

Notice that as you start working, the Notebook prompts you with several autocomplete options. Use the RAML definition hosted at DropBox for Instagram. 

```
API.createClient('instagram', 'https://dl.dropboxusercontent.com/u/3537856/instagram-v1/api-single.yml');
```

Upon entering this command into the Notebook, and hitting enter to "play" the cell, the server will return a number of functions that are now available to you. You can review locations, media, tags, and users. You can investigate oEmbed, and you see that authentication is also available.

Step 2: Authenticate

Authenticate with OAuth 2.0 sooner rather than later. You'll need to register any new application at http://instagram.com/developer with the redirect_uri {APINotebookHost}/authentication/oauth2.html. Insert the client ID and secret given to you into a code cell as follows and run the cell: 

```
instagram.authenticateOauth20 ({clientId: 'clientID', clientSecret: 'secret'});
```

If successful, an auth dialog window will open so you can log in with valid Instagram user credentials.

The Notebook delivers the request to the server for you, and returns your accessToken. The Notebook works sequentially: because you've entered in this request up here, every call you make to the server from here on out will now incorporate the required security information.

Now let's make a request to the server. The hard part is done and immediately you are 80% of the way to creating a use-case. 

What if you wanted to see what's popular on Instagram? Autocompletion takes you most of the way there:

```
instagram.media.popular.get();
```

Not only is the Notebook useful for testing out an API that you are thinking of using, it's also handy for testing an API that you are in the process of writing. The Notebook can be as collaborative as you want - simply share the URL. Once you make your own copy (see top right hand corner), every change you make is recorded in your gists on Github. This means that you can return to your work by opening a saved Gist in the Notebook. To enable changes: authenticate with Github, then click "make my own copy". You just forked the original notebook, and are working in your own copy. It's autosaved to your github gists repo with the id located after the # in your URL.
