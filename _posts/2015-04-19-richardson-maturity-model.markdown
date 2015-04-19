---
layout:     post
title:      "The Richardson Maturity Model"
date:       2015-04-19 13:30:00
categories: general
---
The Richardson Maturity Model (RMM) describes the level of friendliness, or how close your web services are to the [REST][rest] architecture. In other words, how far your API is from the benefits of [REST][rest]:

  - scalability
  - performance
  - loose coupling
  - consistency/maintainability

The RMM is divided into 4 levels (0 - 3), where 3 is the maximum score: a truly RESTful API.

## Level 0 - Tunnelling
This level includes all those services that use only one method of the protocol, and only one entry point. In [HTTP][http], this means sending requests and responses to the service using one of the verbs (usually POST) to an [URI][uri]. Examples of Level 0 web services are [SOAP][soap] and [XML-RPC][xml-rpc].

## Level 1 - Resources
If a web service provides different end points for different resources, then we can say that it meets level 1 of RMM. For example, instead of having a unique end point: ``http://server.org/users``, the service provides different end points for different users: ``http://server.org/user/1`` ``http://server.org/user/2``.

## Level 2 - Verbs
This level brings protocol verbs to the table. Talking about [HTTP][http], these are GET, POST, PUT, DELETE, OPTIONS, HEAD, TRACE, CONNECT and PATCH. That means that the web service uses protocol properties to interact with resources. For example,  a GET request to ``http://server.org/user/1`` retrieves information about user one, and a DELETE request to ``http://server.org/user/1`` removes the resource.

Another feature included in level 2 are [response codes][response_codes]. These codes provide information about the status of a request. For example, 201 when a new resource is created.

## Level 3 - Hypermedia controls
Finally, at level 3 the API includes [HATEOAS][hateoas] in order to discover which next actions can be requested for a client. For example, after a POST request to ``http://server.org/user/1`` which creates a new resource, the response will include a link to a GET ``http://server.org/user/1`` request. This means that retrieving the created user is one of the next valid actions for the client, and the provided link is the way to trigger it.

For further reading:

- [Martin Fowler: Richardson Maturity Model][fowler]
- [The RESTful CookBook][cook_bbok]
- [REST in Practice: Hypermedia and Systems Architecture][rest_book]

[rest_book]:http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829/ref=sr_1_1?s=books&ie=UTF8&qid=1429442007&sr=1-1&keywords=rest+in+practice
[rest]:http://en.wikipedia.org/wiki/Representational_state_transfer
[hateoas]:http://en.wikipedia.org/wiki/HATEOAS
[http]: http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
[soap]:http://en.wikipedia.org/wiki/SOAP
[xml-rpc]:http://en.wikipedia.org/wiki/XML-RPC
[uri]:http://en.wikipedia.org/wiki/Uniform_resource_identifier
[response_codes]:http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html
[fowler]:http://martinfowler.com/articles/richardsonMaturityModel.html
[cook_bbok]:http://restcookbook.com/Miscellaneous/richardsonmaturitymodel
