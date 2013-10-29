Designer

### Assumption:
You have working knowledge of RAML, having worked through the tutorials available at raml.org, and are ready to start designing your API. You don't necessarily have expertise in either RAML or RESTful API practices, but you have some technical knowledge.

### What is the Designer?
The Designer is the custom environment specifically dedicated to designing and writing an API definition in RAML. It is the creation point for your API.

### Where can I find the Designer?
The code for the Designer is available on Github and at raml.org. We also encourage you to directly enter your RAML API definition in the API Designer at [APIhub.com](http://www.apihub.com) to immediately expose it to the larger developer community. You will find the Designer paired with the Console, so that you can immediately see how your API is taking shape as you write it. 

### When do I use the Designer?
At design time - when you are ready to write your API.

### Why should I use the Designer?
When it comes time to actually write your API, RAML helps you transfer your mental API construct into an actual definition. The Designer helps you focus on writing your API without getting bogged down in code. While you can write RAML in an application as simple as a text editor, the Designer provides several advantages. **Syntax highlighting** keeps your API easy to read. The shelf at the bottom of the Designer serves as a **suggestion engine** for what should come next in your API's hierarchy. The best part: **no invalid RAML!** The Designer immediately alerts you when you make a mistake.  Create your API in the Designer to see it come to life in the Console - interactive documentation for your API consumers. 

### How do I use the Designer?
When entering long form documentation (i.e. an authentication process) the designer accepts standard Markdown syntax. To keep your API definition as concise as possible for your consumers, we encourage you to take advantage of RAML's include! property to host documentation, schemas, and often-used patterns outside the definition itself. The Designer's parser interprets include! as if the content of the externally-hosted file were declared in-line.

Now, start writing your API! 

#### Step 1: The Root
Make sure the first line begins with the correct version. Right now, that's 0.8:

```
#%RAML 0.8
```

Look to the Shelf at the bottom of the Designer. There you'll see a number of categories: Root, Parameters, Security, Resources, Traits and Types.

Each category defines what you are able to enter at that point in the hierarchy of your API. Begin with the root: it contains information that will apply to the rest of your API definition. For example, naming and versioning your API are important. Click the first two options under Root, *title* and *version*, to automatically add them to your definition so that it now looks like so:

```
#%RAML 0.8
title: I <3 APIs
version: v0.1
```

As you select options to appear in your definition, they will disappear. You won't need to re-use title or version, as they only appear once in a definition anyway: fewer choices mean fewer mistakes! 

If you have any baseUri parameters or pre-determined security schemes, enter them at this level. You can learn more about how a RAML API definition includes security settings [here](https://github.com/raml-org/raml-spec/blob/master/08_security.md) in the spec.

```
#%RAML 0.8
title: I <3 APIs
version: v0.1
baseUri: https://api.iheartthem.com/{version}/
mediaType: application/json
```

Click the shelf to enter the *baseUri*, and update the provided template with your API's URI, required for a valid RAML definition. 

Traits and resource types are also entered at the root of the API definition since they are repeatedly referenced throughout your API. RAML leverages the patterns in your API: can you think of any to enter right now? Remember, you can always return to the root of your API when you have a few resources and methods sorted out. In fact, we strongly recommend that you write your API and discover patterns as they emerge, rather than trying to predict behavior. This keeps your API organized and concise. Check out the level 200 tutorial for more tips on how to create traits and resource types.

#### Step 2: Resources

Next, add a new resource. Click *New Resource* directly from the Shelf to get started with a space to enter both the resource itself and the displayname for the interactive documentation in the Console. Let's call this first resource "Users". Hit enter once you've updated the displayName with "Users". The Designer indents the next line for you automatically and recognizes that you are now at a different level in the hierarchy with different needs. The shelf changes to reflect your new options: Methods, Parameters, Security, Resources, Traits and Types.

```
#%RAML 0.8
title: I <3 APIs
version: v0.1
baseUri: https://api.iheartthem.com/{version}/
mediaType: application/json
/Users:
  displayName: Users
```

#### Step 3: Methods

Now add methods and parameters to your API at this level. Note that when you indent to add queryParameters, the shelf disappears because it cannot predict what parameters you need! However after you've named the queryParameter, you've moved on to the next line and indented appropriately, the shelf reappears with new options: *Docs* and *Parameters*. Click to enter characteristics you require: 

```
#%RAML 0.8
title: I <3 APIs
version: v0.1
baseUri: https://api.iheartthem.com/{version}/
mediaType: application/json
/Users:
  displayName: Users
  get:
    description: Get a list of users
    queryParameters:
      firstName:
        type: string
        required: false
      lastName: 
        type: string
        required: true
      userId:
        type: integer
        required: true
```

If you've defined a security scheme at the root, use "securedBy" and name the scheme the resource requires in an array. Similarly, use "is" to reference a trait you've described in the root, and "type" to reference a resource type. Refer to the tutorials provided at raml.org for further use cases for each category on the shelf. 