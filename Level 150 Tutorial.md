### Objective: 
Learn how to expand your resource definitions in RAML with request headers, request bodies, and responses.

### Introduction: 
An API definition is not complete without details predicting and detailing API behavior. Request headers, bodies, and responses provide a framework for including additional information you want carried with calls to and from your API. 

### Assumptions: 
You know how to specify the components of a RESTful API, and have progressed through the Level 100 tutorial. As you work through this tutorial, add the code to your existing API definition from level 100.

### Step 1: Enter Request Headers
Part of your API definition for the BookMobile API includes non-standard HTTP headers. The headers property in RAML functions as a standard key:value pair, and defines the operating parameters of your calls to the API. Headers define metadata for the request, give identification to a project, and often manage security credentials. 

Return to your e-bookmobile API, and create a header for a POST request to the resource /shelves. Through this definition of POST, an “x-bookmobile-date” element is created so that the server can keep track of when POST calls are made.

```yaml
#%RAML 0.8
---
title: e-BookMobile API
baseUri: http://api.e-bookmobile.com/{version}
version: v1
/shelves:
  /{shelfName}:
    post:
      summary: Add a shelf
      headers:
        x-bookmobile-date:
          displayName: Time stamp
          description: Specifies  a timestamp for authenticated requests
          type: date
          example: Sun, 06 Nov 1994 08:49:37 GMT
```

Now let’s say you want to give your user the ability to copy a shelf and all of its associated content. For example, the user, nytFaves has a shelf of {cookbooks}. You’d like to add create your own shelf, {food} that contains everything on the {cookbooks} shelf but also has room for Ruth Reichl’s Garlic and Sapphires. Add a header with a custom name that points to the source of a copied shelf with x-bookmobile-copy-source-{*}, i.e. the original user. You can define multiple custom named headers in RAML by using the {*} as a placeholder for 0 or more valid characters in the given name of the header, offering a way for implementations to add any number of headers that fit the naming convention. 

Enter the following example into your API definition:

```yaml
/shelves:
  /{shelfName}:
    post:
      summary: Add a shelf
      headers:
        x-bookmobile-date:
          displayName: Time stamp 
          description: Specifies  a timestamp for authenticated requests
          type: date
          example: Sun, 06 Nov 1994 08:49:37 GMT
        x-bookmobile-copy-source-{*}
          description: Field names prefixed with x-bookmobile-copy-source- specify the source bucket and object for a copy operation
          example: x-bookmobile-copy-source-nytFaves:cookbooks
```

Notice that both headers include an example attribute, which is required for all headers.

### Step 2: Enter the Body
PUT, POST, and PATCH expect the resource to be sent as part of the request body. Makes sense - to create or add an object, you must send the relevant content with your call to the server. RAML allows for several types of media input to format your content. 

Suppose that you only want your consumers to pass JSON in the request body of the POST call. The example below defines a JSON media input for the body of your request, restricting API consumers from trying to sneak in XML.

```yaml
/shelves:
  /{shelfName}:
    post:
      summary: Add a shelf
      headers: …
      body:
        application/json:
```

However, if you'd like to utilize web forms instead, RAML requires special encoding and declarations with the formParameters property. In the below example you can see this property nested below the assigned web form (application/x-www-form-urlencoded).  Form parameters are a key: value pairing that modify the web form of your request body.

```yaml
/shelves:
  /{shelfName}:
    post:
      summary: Add a shelf
      headers: …
      body:
        application/x-www-form-urlencoded:
          formParameters:
```

Add an email address as a form parameter for a x-www-form-urlencoded web form so that any user trying to add a shelf must also include their email address:

```yaml
/shelves:
  /{shelfName}:
    post:
      summary: Add a shelf
      headers: …
      body:
        application/x-www-form-urlencoded:
          formParameters:
            email_address:
              description: The email address to use as the identifier for shelf-creation.
              type: string
              required: true
              example: bobs_donuts@aol.com
```

### Step 3: Enter Responses
Your RAML definition for the BookMobile API also defines responses for its resource methods. You can build your response definitions with description, example attributes, and schema properties.

Add a 200 OK response to your GET request retrieving a specific bookseller affiliated with e-BookMobile:

```yaml
/booksellers:
  /{booksellerId}:
    displayName: Bookseller
    get:
      description: Retrieve a specific bookseller
      responses:
        200:
          description: OK
          body:
            application/json:
            example: !include examples/bookmobile-v1-bookseller.json
```

Add as many responses as you need, but responses only use HTTP status codes. You can also group an array of HTTP status codes together. 

Add the 302 response to your resource definition, because you want to indicate that the server found the requested information:

```yaml
/booksellers:
  /{booksellerId}:
    displayName: Bookseller
    get:
      description: Retrieve a specific bookseller
      responses:
        [302, 204]:
          body:
            application/json:
              example: !include examples/bookmobile-v1-bookseller.json
```

### Step 4: Enter Response Headers
While RAML uses standard HTTP status codes to describe responses, it does accept associated non-standard HTTP headers (just like request headers). 

What if your API consumer has created a gorgeous and popular application using your thoughtfully-designed API, and exceeds the limits you've set? They'd receive a 503 response. 

Add a waiting period header to the 503:

```yaml
/books:
  /bestsellers:
    get: 
      description: Get a list of current NYT bestsellers.
      responses:
        503: 
          description: The service is currently unavailable or you exceeded the maximum requests per hour allowed to your application.
          headers:
            x-waiting-period:
              description: The number of seconds to wait before you can attempt your request again.
              type: integer
              required: yes
              minimum: 1
              maximum: 3600
              example: 34
```

The naming convention for response headers works the same way as it does for request headers, only instead of using {*} as a placeholder, RAML uses {?} as a stand-in for further characters appended to the name of the response header. 

Add the x-meta prefix header to the GET bestsellers method definition for a 200 response .

```yaml
/books:
  /bestsellers:
    get: 
      description: Get a list of current NYT bestsellers.
      responses:
        200:
          description: The list of current NYT bestsellers.
          headers:
            x-meta-{?}:
              description: Field names with this prefix contain user-specified metadata.
```

Now that you have a solid grounding for adding information in the form of headers, bodies, and responses, go back to the API definition you designed during the level 100 tutorial and build it out further with defined requests and responses. Next up in the level 200 tutorial: RAML powers pattern definitions. 
