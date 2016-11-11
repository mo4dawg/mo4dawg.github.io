---  
layout: post  
title: "Restful Services"  
date: 2016-11-11  
tags:  
- career  
published: true  
--- 
So I've been getting myself updated on REST services.  Over the past few months I have revisited the topic several times and have seen numerous You Tube videos on Restful service development.   I decided to create this page simply as a go to for myself so I admit to about 60 percent plagiarism on the below. However that means about 40% are my own thoughts and words. :-) 

**Primary Tenants of REST**

REST is short for REpresentational State Transfer.

REST is an amazingly simplified way of approaching service oriented architecture as opposed to the maintenance of the WSDL, XSD, and version control of SOAP services. 

**Resource-Based** 
Individual resources are identified in requests using URIs as resource identifiers. REST is resource based vs action based or noun based vs verb based.  SOAP tends to have operations that are verb based methods. 
An important aspect of resource based URIs is the notion of a collection vs a record.  A collection should be expressed as a plural 
Collection: *GET http://www.example.com/customers*
Record:     *GET http://www.example.com/customers/12345*

**Manipulation of Resources Through Representations**
When a client holds a representation of a resource, including any metadata attached, it has enough information to modify or delete the resource on the server, provided it has permission to do so.  By representations it is meant a representation of the application state.  For example, the server does not send its database, but rather some XML or JSON that represents some database records.

**Self-descriptive Messages**
Each message includes enough information to describe how to process the message. Responses also explicitly indicate their cache-ability. 
Hypermedia as the Engine of Application State (HATEOAS)

Clients deliver state via body contents, query-string parameters, request headers and the requested URI. Services deliver state to clients via body content, response codes, and response headers. This is technically referred-to as hypermedia (or hyperlinks within hypertext).  HATEOS also means that, where necessary, links are contained in the returned body (or headers) to supply the URI for retrieval of the object itself or related objects.

**The six constraints of REST are:**

**1.   Uniform Interface**

The uniform interface constraint defines the interface between clients and servers. It simplifies and decouples the architecture, which enables each part to evolve independently. 

Most organizations use the HTTP protocol with its well defined API although HTTP in itself is not a constraint.  For CRUD operations: 
  **POST** – use for Create.  Post is neither safe nor idempotent.  Posting the same information twice will likely result in two of the same resources being created. 
  **GET** – used for Read operations.  This operation is both safe and idempotent 
  **PUT** – used for Update.  PUT is safe in that if you update an item twice with the same information there is no change.  So if you update a resource from a value of 3 to 4 twice the result is one resource with its value being 4.  However if you were to POST a resource that has a value 3 twice with a value of 4 you will update the resource to 4 but also create a new resource with a value of 4. 
  **DELETE** - is just that.  It is safe in that you can call DELETE on a resource more than once without impact.  You would get an HTTP response 200 (OK) on the first call and a response 404 (not found) on the second call. 
  
**2.   Stateless**

As REST is an acronym for REpresentational State Transfer, statelessness is key. Essentially, what this means is that the necessary state to handle the request is contained within the request itself, whether as part of the URI, query-string parameters, body, or headers. The URI uniquely identifies the resource and the body contains the state (or state change) of that resource. Then after the server does it's processing, the appropriate state, or the pieces of state that matter, are communicated back to the client via headers, status and response body.

In REST, the client must include all information for the server to fulfill the request, resending state as necessary if that state must span multiple requests. Statelessness enables greater scalability since the server does not have to maintain, update or communicate that session state. Additionally, load balancers don't have to worry about session affinity for stateless systems. 

State, or application state, is that which the server cares about to fulfill a request—data necessary for the current session or request. A resource, or resource state, is the data that defines the resource representation—the data stored in the database, for instance. Consider application state to be data that could vary by client, and per request. Resource state, on the other hand, is constant across every client who requests it.

**3.   Cacheable**

As on the World Wide Web, clients can cache responses. Responses must therefore, implicitly or explicitly, define themselves as cacheable, or not, to prevent clients reusing stale or inappropriate data in response to further requests. Well-managed caching partially or completely eliminates some client–server interactions, further improving scalability and performance.

**4.    Client-Server**

The uniform interface separates clients from servers. This separation of concerns means that, for example, clients are not concerned with data storage, which remains internal to each server, so that the portability of client code is improved. Servers are not concerned with the user interface or user state, so that servers can be simpler and more scalable. Servers and clients may also be replaced and developed independently, as long as the interface is not altered.

**5.   Layered System** 

A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way. Intermediary servers may improve system scalability by enabling load-balancing and by providing shared caches. Layers may also enforce security policies.

**6.  Code on Demand (optional)**

Servers are able to temporarily extend or customize the functionality of a client by transferring logic to it that it can execute. Examples of this may include compiled components such as Java applets and client-side scripts such as JavaScript.

Complying with these constraints, and thus conforming to the REST architectural style, will enable any kind of distributed hypermedia system to have desirable emergent properties, such as performance, scalability, simplicity, modifiability, visibility, portability and reliability. 
