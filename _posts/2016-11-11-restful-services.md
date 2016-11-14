---  
layout: post  
title: "Restful Services"  
date: 2016-11-11  
tags:  
- career  
published: true  
--- 
So I've been getting myself tuned up on Restful services. &nbsp;  At Nordstrom our teams started the SOA path with SOAP but by 2014 had moved pretty much entirely to Rest except for the now legacy services.  &nbsp; I hate the fact that all the services we wrote for PBR (Our CRM tool rewrite) were SOAP but it was the timing.  &nbsp;We were learning service oriented architecture and SOAP was our go to service at the time.  &nbsp;Over the past few months I have revisited the topic several times and have visited numerous sites on Restful service development.    &nbsp;I decided to create this page simply as a go to for myself so I admit to about 60 percent plagiarism on the below.  &nbsp; However that means about 40% are my own thoughts and words. :-) 

***Primary Tenants of REST***

REST is short for REpresentational State Transfer.

REST is an amazingly simplified way of approaching service oriented architecture as opposed to the maintenance of the WSDL, XSD, and version control of SOAP services. 

**Resource-Based** 

Individual resources are identified in requests using URIs as resource identifiers.  &nbsp;REST is resource based vs action based or noun based vs verb based.  SOAP tends to have operations that are verb based methods. 

An important aspect of resource based URIs is the notion of a collection vs a record.  &nbsp; A collection should be expressed as a plural.

Collection: *GET http://www.example.com/customers*

Record:     *GET http://www.example.com/customers/12345*

**Manipulation of Resources Through Representations**

When a client holds a representation of a resource, including any metadata attached, it has enough information to modify or delete the resource on the server, provided it has permission to do so.  &nbsp; By representations it is meant a representation of the application state.  &nbsp; For example, the server does not send its database, but rather some XML or JSON that represents some database records.

**Self-descriptive Messages** 

Each message includes enough information to describe how to process the message. &nbsp; Responses also explicitly indicate their cache-ability. 

**Hypermedia as the Engine of Application State (HATEOAS)** 

Clients deliver state via body contents, query-string parameters, request headers and the requested URI. &nbsp; Services deliver state to clients via body content, response codes, and response headers. &nbsp; This is technically referred-to as hypermedia (or hyperlinks within hypertext). &nbsp;  HATEOS also means that, where necessary, links are contained in the returned body (or headers) to supply the URI for retrieval of the object itself or related objects.

**Definitions of Safe and Idempotent**

**Safe** &nbsp;&nbsp;Safe methods are HTTP methods that do not modify resources.&nbsp; For instance, using GET or HEAD on a resource URL, should NEVER change the resource.&nbsp; However, this is not completely true.&nbsp; It means: it won't change the resource representation.&nbsp; It is still possible, that safe methods do change things on a server or resource, but this should not reflect in a different representation.

**Idempotent** &nbsp;&nbsp;An idempotent HTTP method is a HTTP method that can be called many times without different outcomes.&nbsp; It would not matter if the method is called only once, or ten times over.&nbsp; The result should be the same. &nbsp;This only applies to the result, not the resource itself.&nbsp; This still can be manipulated (like an update-timestamp, provided this information is not shared in the (current) resource representation.

The key thing to understand is that POST is not idempotent but PUT is.  &nbsp;So if you call PUT with the same arguments the result is always the same. &nbsp; For POST this is not guarenteed. &nbsp; Here is an important consideration and why you shoule reserve POST to the Create operation: 

What would happen if you sent out the POST request to the server, but you get a timeout.&nbsp; Is the resource actually updated?&nbsp; Does the timeout happened during sending the request to the server, or the response to the client ?&nbsp; Can we safely retry again, or do we need to figure out first what has happened with the resource? &nbsp; By using idempotent methods, we do not have to answer this question, but we can safely resend the request until we actually get a response back from the server.

A good way to look at idempotency is to take these two operations:&nbsp; *x = 4;*  &nbsp;  This is idempotent because x will always be 4 no matter how many ties it is called. &nbsp; But consider *i++*. &nbsp;  This will increment every call so by definiton is not idempotent.

***The six constraints of REST are:***

**1.   Uniform Interface**

The uniform interface constraint defines the interface between clients and servers. &nbsp; It simplifies and decouples the architecture, which enables each part to evolve independently. 

Most organizations use the HTTP protocol with its well defined API although HTTP in itself is not a constraint. &nbsp;  For CRUD operations: 

 &nbsp; &nbsp;  **POST** – use for Create.  &nbsp; Post is neither safe nor idempotent.  &nbsp; Posting the same information twice will likely result in two of the same resources being created. 
  
 &nbsp; &nbsp;  **GET** – used for Read operations. &nbsp;  This operation is both safe and idempotent 
  
 &nbsp; &nbsp;  **PUT** – used for Update. &nbsp;Since PUT updates it by definition is not Safe. &nbsp;  PUT is idempotent in that if you update an item twice with the same information there is no change.  &nbsp; So if you update a resource from a value of 3 to 4 twice the result is one resource with its value being 4. &nbsp;  However if you were to POST a resource that has a value 3 twice with a value of 4 you will update the resource to 4 but also create a new resource with a value of 4. 
  
  &nbsp; &nbsp; **DELETE** - is just that.  &nbsp; It is safe in that you can call DELETE on a resource more than once without impact.  &nbsp; You would get an HTTP response 200 (OK) on the first call and a response 404 (not found) on the second call. 
  
**2.   Stateless**

As REST is an acronym for REpresentational State Transfer, statelessness is key. &nbsp;  This means that the necessary state to handle the request is contained within the request itself, whether as part of the URI, query-string parameters, body, or headers. &nbsp; The URI uniquely identifies the resource and the body contains the state (or state change) of that resource. &nbsp; Then after the server does it's processing, the appropriate state, or the pieces of state that matter, are communicated back to the client via headers, status and response body.

In REST, the client must include all information for the server to fulfill the request, resending state as necessary if that state must span multiple requests. &nbsp; Statelessness enables greater scalability since the server does not have to maintain, update or communicate that session state. &nbsp; Additionally, load balancers don't have to worry about session affinity for stateless systems. 

State, or application state, is that which the server cares about to fulfill a request—data necessary for the current session or request. &nbsp; A resource, or resource state, is the data that defines the resource representation—the data stored in the database, for instance. &nbsp; Consider application state to be data that could vary by client, and per request. &nbsp; Resource state, on the other hand, is constant across every client who requests it.

**3.   Cacheable**

Clients can cache responses. &nbsp; Responses must therefore, implicitly or explicitly, define themselves as cacheable, or not, to prevent clients reusing stale or inappropriate data in response to further requests. &nbsp; Well-managed caching partially or completely eliminates some client–server interactions, further improving scalability and performance.

**4.    Client-Server**

The uniform interface separates clients from servers. &nbsp; This separation of concerns means that, for example, clients are not concerned with data storage, which remains internal to each server, so that the portability of client code is improved.  &nbsp;Servers are not concerned with the user interface or user state, so that servers can be simpler and more scalable. &nbsp; Servers and clients may also be replaced and developed independently, as long as the interface is not altered.

**5.   Layered System** 

A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way.  &nbsp;Intermediary servers may improve system scalability by enabling load-balancing and by providing shared caches. &nbsp; Layers may also enforce security policies.

**6.  Code on Demand (optional)**

Servers are able to temporarily extend or customize the functionality of a client by transferring logic to it that it can execute. &nbsp; Examples of this may include compiled components such as Java applets and client-side scripts such as JavaScript.

Complying with these constraints, and thus conforming to the REST architectural style, will enable any kind of distributed hypermedia system to have desirable emergent properties, such as performance, scalability, simplicity, modifiability, visibility, portability and reliability. 
