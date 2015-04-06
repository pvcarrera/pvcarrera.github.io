---
layout:     post
title:      "The web as a platform for building distributed systems"
date:       2015-04-06 21:20:00
categories: general
---
The title and most of the content of this post has been extracted from the book [REST in practice: Hypermedia and Systems Architecture][rest_book]. In it, I found a nice description of the Web architecture, one I'd like to summarise and share here.

The authors present the Web as an example of a successful distributed system,  and an inspiration to build new ones. They also describe the key concepts or building blocks of the Web architecture:

## Resources and identifiers
The fundamental building blocks of the Web are resources, because the Web is a resource-oriented architecture. A resource is anything a consumer/client interacts with while progressing towards a goal.

The mechanisms the Web provides to identify and consume resources are the [URI][uri] and [HTTP protocol][http].

A resource can have more than one [URI][uri], but not the other way around, e. g. www.google.com / google.com

## Resource representation
A resource representation is a transformation or a view of a resource in a specific point in time. A resource can have multiple representations. For example, the resource request ``http://server/users/1`` is going to show the state of user 1 in the moment you make the request, and its representation can be a JSON, XML, JPG, etc. response

## Representation formats and URIs
A common misconception is thinking that a representation matches a [URI][uri]. For example, if I want the xml representation of my user I'd ask for ``http://server/users/1.xml``, and if I want the JSON representation I'd ask for ``http://server/users/1.json``. This isn't correct, URIs should be opaque to the consumer.

The solution to this problem is using content negotiation, so clients can ask for a specific representation format using [HTTP headers][headers]. For example, I can add the Accept header to the ``http://server/users/1`` request with the value `application/json` or `application/xml` value.

## Interacting with resources
The mechanism that [HTTP][http] provides to interact with resources are the HTTP verbs: GET, POST, PUT, DELETE, OPTIONS, HEAD, TRACE, CONNECT and PATCH. So, given a resource, we can use one of these verbs to interact (trigger an action) with it.

In addition to the verbs, [HTTP][http] provides a set of response codes: ``200 OK``, ``404 Not Found``, etc. to give additional information on the result of the action that's been triggered by the [HTTP][http] verb on a resource.

With this post I'm breaking [the rule of two pomodoros][welcome], so that's it for today.

[rest_book]: http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829

[welcome]: {% post_url 2014-11-29-welcome %}
[uri]: http://en.wikipedia.org/wiki/Uniform_resource_identifier
[http]: http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
[headers]: http://en.wikipedia.org/wiki/List_of_HTTP_header_fields

