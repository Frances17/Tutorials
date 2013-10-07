### Objective: 
Learn how to leverage RAML's unique ability to allow you to define patterns to design a concise and straightforward API. You might even recognize some YAML tricks!

### Introduction: 
Any smart and impatient human is frustrated with dull repetition. RAML's philosophies <<link out to relevant page>> endeavor to address this through pattern recognition. RAML works to be human and machine readable while cutting down on redundancy and complexity to reduce errors. Most importantly, it allows the API designer to recognize structural patterns in the API (across schemas, resources, or methods) and describe these patterns at the root so that they can apply to the entire API. Define it once, re-use many times.

### Assumptions: 
You know how to specify the components of a RESTful API, and have progressed through the Level 100 and 150 tutorials.

### Step 1: Enter Schemas

Let's start by defining some schemas for your BookMobile API. Remember - a schema is like a template that defines sets of attributes that can be used to describe resources. 

RAML lets you use JSON or XML for schemas. Go ahead and build out a few JSON request schemas for a submitted book review and a bookseller:

```yaml
#%RAML 0.8
---
title: e-BookMobile API
baseUri: http://api.e-bookmobile.com/{version}  
version: v1
schemas:
 - review: |
    {  "$schema": "http://json-schema.org/draft-03/schema",
       "type": "object",
       "description": "A book review",
       "properties": {
         "title":  { "type": "string" },
         "description":  { "type": "string" },
         "fileUrl":  { "type": "string" },
         "region": { "type": "string" }
                     }
   "required": [ "title", "author" ]
    }
 - bookseller: |
    {  "$schema": "http://json-schema.org/draft-03/schema",
       "type": "object",
       "description": "An affiliated bookseller",
       "properties": {
         "name":  { "type": "string" },
         "contact":  { "type": "string" },
         "region": { "type": "string" }
                     }
   "required": [ "first name", "last name" ]
    }
```

These will come in handy when we define those resources more explicitly beyond just their attributes.

In the real world, you will likely need to define more than 2 schemas. What does your API start to look like at 15 schemas? That's a lot of lines of code! Use the !include property to link to external files, like so:

```yaml
#%RAML 0.8
---
title: e-BookMobile API
baseUri: http://api.e-bookmobile.com/{version}  
version: v1
schemas:
  !include: review_schema.json
  !include: bookseller_schema.json
```

This is also useful for chunks of documentation that you'd like to include in your API definition, such as a getting started section. RAML consistently returns to the theme of "just what you need, and nothing more". 

### Step 2: Enter Traits

Another key way RAML defines patterns is with traits. A trait is a partial method definition that can provide method-level properties such as descriptions, headers, query string parameters, and responses. A resource may apply one or more traits, and inherit all the properties of those traits.   
As with schemas, both traits and resource types are declared at the root level, as they both source information that will be available to the rest of the API. 

Let's begin with traits:

```yaml
#%RAML 0.8
---
title: e-BookMobile API
baseUri: http://api.e-bookmobile.com/{version}  
version: v1
schemas: 
- review: ...
- bookseller: ... 
traits:
```

What are some instances that come to mind when thinking about API patterns? Securing your API is pretty common - API providers can provide a required access_token query parameter to be passed along with each call. What does that look like when applied to the root of the bookmobile API as a header?

```yaml
traits: 
- secured:
    displayName: Secured
    headers:
      authorization:
        description: The auth token for this request
    responses: 
      401:
        description: Unauthorized
```

The “secured” trait consists of everything nested under it: both headers and responses.

Another classic example is splitting the returned results from a GET request into pages, this is often necessary when the output may be too large for the client to handle all at once. Can you develop the “paged” definition?

```yaml
traits:
- paged:
    displayName: Paged
    queryParameters:
      start: 
        displayName: Start
        description: The first page to return
        type: number
      pages:
        displayName: Pages
        description: The number of pages to return
        type: number
```

Again, because you've defined these traits in the root, when you write the full API, resources and all, you can reference these traits inside square brackets to abbreviate your resource definition to:

```yaml
/resource:
  is: [ secured, paged ]
```

### Step 3: Enter Resource Types

RAML also uses resource types. A resource type is a partial resource definition that can specify a description and methods and their properties. A resource will only inherit the properties of one resource type.

So, let's say there are a couple of things that we can assume about every single resource and associated method for BookMobile API. Call this the base, and enter the following into your API definition under the root:

```yaml
resourceTypes:
 - base:
```

You are just beginning to design your API, so there are still other things that you don’t know if they’ll be applicable to every single resource. You can use optional structure keys for properties that MIGHT show up at any level:

```yaml
resourceTypes:
  - base:
      get?:
      put?:
      patch?:
      post?:
      delete?:
```

Optional structure keys help you cover all your bases. For example, what will you receive with every successful call? A 200 response.

```yaml
resourceTypes:
  - base:
    get?:
      responses: &standardResponses
        200:
          description: OK
```

&standardResponses serves as an anchor for another, in-line pattern definition exercise: it anchors everything nested under it (in this case, the 200 and the description). A 200 response is a default good response, it makes sense to apply it to the rest of your potential methods by using *standardResponses:

```yaml
resourceTypes:
  - base:
    get?: 
      responses: &standardResponses
        200:
          description: OK
    put?:
      responses: *standardResponses
    patch?: 
      responses: *standardResponses
    post?: 
      responses: *standardResponses
    delete?:
      responses: *standardResponses
```

You've essentially abbreviated the 200 response nested under &standardResponses to *standardResponses. Recognize this YAML trick? If you were to expand this base resource type, it would look like this:

```yaml
resourceTypes:
  - base:
    get?:
      responses: 
        200:
          description: OK
    put?:
      responses: 
        200:
          description: OK
    patch?:
      responses: 
        200:
          description: OK
    post?:
      responses: 
        200:
          description: OK
    delete?:
      responses: 
        200:
          description: OK
```

Note: this is perfectly acceptable. Anchors and references with & and * just serve to deliver a much more concise API.

Recall the resources you designed in the Level 100 tutorial: users, authors, and books. Those all sound like collections of objects - another pattern! 

Your collections will definitely all need 200 responses, so your new “collections” resource type can be itself defined as a resource of type “base”. Define it like this in your API definition:

```yaml
resourceTypes:
  - collection:
      type: { base }
      get:
        is: [ paged ]
```

You also know that you want to be able to ADD to the various collections. Define a POST method:

```yaml
resourceTypes:
  - collection:
      type: { base }
      get:
        is: [ paged ]
      post: 
        responses:
          201:
            description: Created
```

Now, what if you are looking at a nested resource - a type of collection? Remember, you've already thought this through in the Level 100 tutorial. Some considerations:

- What kind of patterns would emerge if you created a template for book genres, for example?
- Do you have any traits that you can use?
- Will this use a base resource type pattern definition?
- How far can you define your pattern? Should you define a schema?

```yaml
resourceTypes:
  - typedCollection:
      type: base
      get:
        is: [ paged ]
        responses:
          200: &typed200
          body:
            application/json:
            schema: <<schema>>
```

Your anchored representation (as indicated by &typed200) of what a typical typedCollection 200 response includes a schema. Here, you have not selected a specific schema and instead filled in the space with a variable: <<schema>>.  Remember, at this root level of the API you are still just defining generic patterns. Whenever you implement this pattern (by defining a resource as being of type “typedColllection”), you’re going to have to provide a value for this variable. The variable could be any of the named schemas you detail at the top of the root of the API. So, schema: <<schema>> could be transformed upon implementation into schema: review or schema: bookseller or schema: trashy_romance_novels.

Recalling back to your larger collection resource type, a POST method also makes sense for a typedCollection. Describe the properties of your POST pattern:

```yaml
resourceTypes:
  - typedCollection
      type: base
      get: ...
      post: 
        body: &typedBody
          application/json:
          schema: <<schema>>
   	    responses:
          200: *typed200
```

Again, utilize anchors and references. &typedBody reaches all the way down to include all the nested properties under body, and you can reuse the 200 response with *typed200. 

Let's quickly review what you've already defined:

Schemas:
- review
- bookseller

Responses:
- standard 200 response
- typed 200 response

Body:
- typed body

Traits:
- secured
- paged

Resource types:
- collection
- typed collection
- base

Wow! You've certainly covered a lot of ground! 

Can the patterns you've defined for collection and typed collection help you build out the member and typed member resource types as well? 

```yaml
- member: 
    type: base
    get:
    put:
    patch:
    delete:
- typedMember:
    type: base
    get:
      responses:
        200: *typed200
    put:
      body: *typedBody
      responses:
        200: *typed200
    patch:
      body: *typedBody
      responses: 
        200: *typed200
    delete: {}
```

Remember, as you continue to write out the rest of your API, you can always go back and further define schemas, traits, and resource types as they develop. Then you're free to go back to the business of short-handing the components into your definition. Patterns are more likely to emerge as you design, simply because you are aware of the Power of the Pattern.

### STEP 5: Develop Your Resource

Let's add another resource to your spec:

```yaml
/reviews: &reviews
```

Set up an anchor here, in case you need to refer back to this specific structure. Think ahead: what's the likelihood that you'll need to refer back to the /reviews resource (for a bookseller, or a user)? 

Are there any resource types you can apply here? Would you qualify /reviews as a typedCollection? How would you implement that?

```yaml
/reviews: &reviews
  type: { typedCollection }
```

Review the typedCollection definition from the root of the definition:

```yaml
- typedCollection:
    type: base
    get:
      is: [ paged ]
      responses: 
        200: &typed200
          body: 
            application/json:
            schema: <<schema>>
    post: 
      body: &typedBody
        application/json:
        schema: <<schema>>
      responses:
        200: *typed200
```

Your definition allows for a variable schema, and it makes sense to insert your review schema into your review resource:

```yaml
/reviews: &reviews
  type: { typedCollection: { schema: review } }
```

Now, include a trait:

```yaml
/reviews: &reviews
  type: { typedCollection: { schema: review } }
  is: [ secured ]
```

Next, define a few methods and queryParameters for another resource: /booksellers:

```yaml
/booksellers:
  type: { typedCollection: { schema: bookseller } } 
  is: [ secured ]
  get: 
    queryParameters:
      homeStore:
        displayName: Home Store
        type: string
        description: The store at which the individual user is a bookseller.
```

Think practically. What would it look like if you wanted to get a list of reviews from a specific bookseller?

```yaml
/booksellers:
  type: { typedCollection: { schema: bookseller } }
  is: [ secured ]
  get: 
    queryParameters:
      homeStore:
        displayName: Home Store
        type: string
        description: The store at which the individual user is a bookseller.
  /{booksellerId}:
    type: { typedMember: { schema: bookseller } }
    is: [ secured ]
      /reviews: *reviews
```

Now that you've got a thorough grounding in RAML pattern recognition, go back to the level 100 resources you defined in your spec and see how you can build them out further with what you learned and applied at the root of the e-bookmobile API.


