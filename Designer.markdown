Designer

### Assumption:
You have working knowledge of RAML, having worked through the tutorials available at raml.org, and are ready to start designing your API. You don't necessarily have expertise in either RAML or RESTful API practices, but you have some technical knowledge.

### What is the Designer?
The Designer is the custom environment specifically dedicated to designing and writing an API definition in RAML. It is the creation point for your API.

### Where can I find the Designer?
The code for the Designer is available on Github and at raml.org. We also encourage you to directly enter your RAML definition at APIhub.com to immediately expose it to the larger developer community. You will often find the Designer paired with the Console, so that you can immediately see how your API is taking shape as you design it. Keep in mind that the console works only with a valid RAML spec, helping you catch mistakes in your definition as you proceed.

### When do I use the Designer?
At design time - when you are ready to write your API.

### Why should I use the Designer?
When it comes time to actually design your API, RAML helps you transfer your mental API construct into an actual RAML definition that developers will be able to code JavaScript against in the Notebook. Enter your RAML definition into the Designer to see it come to life in the Console - interactive documentation for your API consumers.

### How do I use the Designer?
Just start writing your RAML definition! When entering long form documentation (i.e. an authentication process) accepts standard Markdown syntax. 

Make sure the first line begins with the correct version. Right now, that's 0.8:

```
#%RAML 0.8
---
```

Look to the Shelf at the bottom of the Designer. There you'll see a number of categories: 

SCREEN SHOT:
ROOT	PARAMETERS	 SECURITY	RESOURCES	TRAITS AND TYPES

Each category defines what you are able to include at that point in the hierarchy of your API. We encourage you to begin with the root: it will contain information that will apply to the rest of your API definition. For example, naming and versioning your API are important. Clicking on the first two options under Root, title and version, will automatically add them to your definition so that it now looks like so:

```
#%RAML 0.8
---
title: I <3 APIs
version: v0.1
```

As you select options to appear in your definition, they will disappear. You won't need to re-use title or version, as they only appear once in a definition anyway: fewer choices mean fewer mistakes!

If you have any baseUri parameters or pre-determined security schemes to include, do so at this level. You can learn more about how a RAML API definition includes security settings <<here in the spec>> and in <<this level 400 tutorial TBD>>.

```
#%RAML 0.8
---
title: I <3 APIs
version: v0.1
baseUri: https://api.iheartthem.com/{version}/
mediaType: application/json
```

Click to enter the baseUri, and update the provided template with your API's URI, required for a valid RAML definition. 

Don't worry about the other categories for now - these are best filled in when you have a few resources and methods already sorted out. We strongly recommend that you write your API and discover patterns as they emerge, rather than trying to predict behavior. This keeps your API organized and concise. Check out the level 200 tutorial for more tips on how to create traits and resource types.

Next, add a new resource. Clicking "New Resource" directly from the Shelf gets you started with a space to both enter the resource itself, and the displayname for the interactive documentation in the Console. Let's call this resource "Collection". Hit enter once you've updated the displayName with "Collection". The Designer recognizes that you are now at a different level in the hierarchy with different needs than you had at the root of the API, and the shelf changes to reflect your new options. 

SCREEN SHOT:
METHODS  	PARAMETERS 		SECURITY 	RESOURCES 	TRAITS AND TYPES

Add methods and parameters to your API at this level. The Designer automatically keeps you to the appropriate tree-like hierarchy: the higher-level the component (like a top-level resource), the further it is to the left. Your first resource is at the top of the hierarchy. 

If you've defined a security scheme at the root, use "securedBy" and name the scheme the resource requires in an array. Refer to the tutorials provided at raml.org for further use cases for each category on the shelf. 