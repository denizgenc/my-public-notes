RESTful API notes
-----
REST == "REpresentational State Transfer"

Rest is Resource Based
-----
- Things instead of actions
- Nouns instead of verbs
    - This is opposed to a SOAP API
    - SOAP would have you do something like "GET user data"
    - RESTful sense would call a URI, and use a HTTP verb to dictate what
      operation we do on that
- Identified by URIs
    - Multiple URIs may refer to same resource
    - We could have a URI that refers to a Person resource, and we can perform
      multiple operations on that, which correspond to HTTP verbs.
- The resources are separate from their representation(s).
    - The representation isn't the resource, it's simply a *representation*

Representations
-----
- This is how resources get manipulated in a RESTful architecture
- They represent part of the resource state
    - They get transferred between a client and server
    - If a client holds a representation of a resource, including any metadata
      or anything else attached, it has enough information to modify or delete
      that resource (as long as the client has permissions to do so)
- Typically JSON or XML (or CSV, or HTML...)
- Example:
    - Resource: person (Todd)
    - Service: contact information (GET)
    - Representation:
        - name, address, phone number
        - JSON or XML format

Uniform Interface
-----
- Defines the interface between the client and server
- Simplifies and decouples the architecture
- Fundamental to RESTful design
- For us this means:
    - HTTP verbs (GET, PUT, POST, DELETE are most common)
    - URIs (Resource name)
    - HTTP response (status, body)
    - (Note RESTful doesn't have to use HTTP, but if we use HTTP as the basis
      for our REST architectures, it makes it simple for us as we already know
      the interface and helps others understand too)

Stateless
-----
- Server contains no client state
- Each request contains enough context to process the message
    - Therefore the messages should be self-descriptive
- Any session state is held on the client
    - The representation is what carries the state.
    - Combining the representation, the HTTP method/verb, and the URI, should be
      enough to contain all of the state for a request 
    
(There are some technologies, such as OAuth 2, which are built in a RESTful
style, but store state on the server - so they are technically not RESTful
services.)

Client-Server
-----
- A RESTful architecture is always a client-server architecture
- Assume a disconnected system
    - The client must understand they won't always have a direct connection to
      (for example) databases or resources
- Separation of concerns
- Uniform interface is the link between the two
    (in our case, the HTTP stack)

Cacheable
-----
- Server responses (representations) are cacheable
    - Implicitly
      - This is if the server doesn't tell the client how to cache the response
    - Explicitly
      - This is when the client defines how to cache the response (max age, etc)
    - or negotiated
      - When the client and server negotiate to figure out how long an item can
        be cached

Layered System
-----
- Client can't assume direct connection to the server
    - For example, a client won't know if a response they got was cached or
      direct (fresh) from the database
- Software or hardware intermediaries between client and server
    - We won't know what the hardware or software used on the server is.
    - All the client should care about is the API, which will stay the same.
- Improves scalability

Code on Demand
-----
- Server can temporarily extend client by transferring logic to the client
- Client executes logic
- Examples:
    - Java applets
    - JavaScript
- The only optional constraint for REST

Summary
-----
- Violating any contraint listed above - apart from Code on Demand - means the
  service is not strictly RESTful (again, an example is "Three-legged" OAUTH2)
- Compliance with REST constraints allows:
    - Scalability
    - Simplicity
    - Modifiability
    - Visibility
    - Portability
    - Reliability

