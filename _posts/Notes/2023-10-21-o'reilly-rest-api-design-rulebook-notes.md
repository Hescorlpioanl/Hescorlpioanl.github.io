---
title: "O'Reilly REST API Design Rulebook Notes"
classes: wide
header:
  teaser: /assets/images/Blogs/Learning-Notes/rest-api-design-rulebook.png
ribbon: ForestGreen
description: "A summary of lessons learned from the O'Reilly REST API Design Rulebook and some Spring Boot implementations"
categories:
  - Notes
tags:
  - Backend
  - Design
toc: true
toc_sticky: true
---

<br>
<div align="center">

بسم الله الرحمن الرحيم <br> احييكم بتحية الإسلام السلام عليكم ورحمة الله تعالى وبركاته

</div>
<br>

# Chapter 01: Introduction

## Hello World Wide Web

In the `Data acquisition and control` group at the **European organization for Nuclear Research (CERN)**, The web starts with a clever programmer who had a great idea for a new
software project.

![Tim photo](https://cds.cern.ch/images/CERN-GE-9407011-31/file?size=large)

In December 1990, `Tim Berners-Lee` started a non-profit software project calles "WorldWideWeb" to facilitate the sharing of knowledge. He worked on this projct for a year and he invented and implemented the following:

- The Uniform Resource Identifier (**URI**)
- The HyperText Transfer Protocol (**HTTP**)
- The HyperText Markup Language (**HTML**)
- The first web server
- The first web browser called **WorldWideWeb** then caleed **Nexua**
- The first **WYSIWYG** HTML editor.

**WYSIWYG** is standing for: What You See Is What You Get.

![the first website on the web](https://cds.cern.ch/images/CERN-HOMEWEB-PHO-2019-004-1/file?size=large)

On August 1991, the web bfgan to grow exponentially, within five years the number of web users skyrocket to 40 million, The number was doubling every two months. The Web was growing too large, too fast, and it was heading to collapse because the web traffic was outgrowing the capacity of the internet infrastructure.in addition to the Web's protocols were not uniformaly implemented and they lacked support for caches and other stablizaing intermediaries.

## Web Architecture

In late 1993, Roy Fielding, co-founder of the Apache HTTP Server Project, Become concerned by the web's scaability problem. After analyzing, Fielding recognized that the web's scaability was governed by a set of key constraints. it was a 6 constraints as the following:

- **Client-Server**
- **Uniform Interface**
- **Layered system**
- **Cache**
- **Stateless**
- **Code-on-demand**

### Client-Server

The separation of concerns is the core theme of the Web’s client-server constraints. The Web is a client-server based system, in which clients and servers have distinct parts to play. They may be implemented and deployed independently, using any language or technology, so long as they conform to the Web’s `uniform interface`.

### Uniform Interface

The interactions between the Web’s components—meaning its clients, servers, and network-based intermediaries—depend on the uniformity of their interfaces.

Web components interoperate consistently within the uniform interface’s four constraints, which Fielding identified as:

1. **Identification of resources**
   Each distinct Web-based concept is known as a resource and may be addressed by a unique identifier, such as a URI
2. **Manipulation of resources through representations**
   Clients manipulate representations of resources. The same exact resource can be represented to different clients in different ways
3. **Self-descriptive messages**
   A resource’s desired state can be represented within a client’s request message. A resource’s current state may be represented within the response message that comes back from a server.
4. **Hypermedia as the engine of application state (HATEOAS)**
   A resource’s state representation includes links to related resources. Links are the threads that weave the Web together by allowing users to traverse information and applications in a meaningful and directed manner.

### Layered System

The layered system constraints enable network-based intermediaries such as proxies and gateways to be transparently deployed between a client and server using the Web’s uniform interface.

### Cache

Caching is one of web architecture’s most important constraints. The cache constraints instruct a web server to declare the cacheability of each response’s data. Caching response data can help to reduce client-perceived latency, increase the overall availability and reliability of an application, and control a web server’s load

A cache may exist anywhere along the network path between the client and server. They can be in an organization’s web server network, within specialized content delivery networks (CDNs), or inside a client itself.

### Stateless

The stateless constraint dictates that a web server is not required to memorize the state of its client applications. As a result, each client must include all of the contextual information that it considers relevant in each interaction with the web server

### Code-On-Demand

The Web makes heavy use of code-on-demand, a constraint which enables web servers to temporarily transfer executable programs, such as scripts or plug-ins, to clients. like the code that run on the old flash player.

## Web Standards

`Fielding` worked alongside `Tim Berners-Lee` and others to increase the Web’s scalability. To standardize their designs, they wrote a specification for the new version of the `Hypertext Transfer Protocol, HTTP/1.1` They also formalized the syntax of `Uniform Resource Identifiers (URI)` in RFC 3986.

## REST

In the year 2000, after the Web’s scalability crisis was averted, Fielding named and described the Web’s architectural style in his Ph.D. dissertation. “`Representational State Transfer`” (REST) is the name that Fielding gave to his description§ of the Web’s architectural style, which is composed of the constraints outlined above.

## REST APIs

Web services are purpose-built web servers that support the needs of a site or any other application. Client programs use `application programming interfaces (APIs)` to communicate with web services.

Having a REST API makes a web service `“RESTful`”. A REST API consists of an assembly of interlinked resources. This set of resources is known as the REST API’s resource model.

**Well-designed REST APIs can attract client developers to use web services. In today’s open market where rival web services are competing for attention, an aesthetically pleasing REST API design is a must-have feature.**

---

# Chapter 02: Identifier Design with URIs

In this chapter, we will talk about the URI design the URIs and its identifiers, and know what is the best practices for it. REST APIs use Uniform Resource Identifiers (URIs) to address resources. The Uniform Resource Identifier (URI): Generic Syntax:

`URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]`

## URIs & Formats

- **Forward slash separator `/` must be used to indicate a hierarchical relationship between resources, Examples:**

  - ✅ `http://api.canvas.restapi.org/shapes/polygons/quadrilaterals/squares`
  - ✅ `http://localhost:8000/api/v1/accounts/1/orders `

- **A trailing forward slash (/) should not be included in URIs, Examples:**

  - ✅ `http://localhost:8000/api/v1/accounts/1/orders`
  - ❌ `http://localhost:8000/api/v1/accounts/1/orders/`

- **Use Hyphens `( - )` instead of Underscores `( _ )` to improve the readability of names in long path segments, Examples:**

  - ✅ `http://api.example.restapi.org/blogs/mark-masse/entries/this-is-my-first-post`
  - ❌ `http://api.example.restapi.org/blogs/mark-masse/entries/this_is_my_first_post`

- **Lowercase letters should be preferred in URI paths**

  | No. | URI                                               | Note                                             |
  | :-: | :------------------------------------------------ | :----------------------------------------------- |
  |  1  | `http://api.example.restapi.org/my-folder/my-doc` | Totaly fine                                      |
  |  2  | `HTTP://API.EXAMPLE.RESTAPI.ORG/my-folder/my-doc` | URI number `2` is the same as the URI number `1` |
  |  3  | `http://api.example.restapi.org/My-Folder/my-doc` | This isn't the same as number `1` or `2`         |

- **File extensions should not be included in URIs. Instead of providing resource extension use the `Accept` request header.**
  - ✅ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall`
  - ❌ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall.json`

## URI Authority Design

- _Consistent subdomain names should be used for your APIs_

The top-level domain and first subdomain names (e.g., `soccer.restapi.org`) of an API should identify its service owner. The full domain name of an API should add a subdomain named api. For example: `http://api.soccer.restapi.org`

- **Consistent subdomain names should be used for your client developer portal**

Use `http://developer.soccer.restapi.org` to help onboard new clients with documentation, forums, and self-service provisioning of secure API access keys

## Resource Archetypes

![Resource Archetypes_pages-to-jpg-0001](https://github.com/0xGhazy/0xGhazy/assets/60070427/f61e51d1-2b79-4b1b-812e-891a17b39f8c)

## URI Path Design

- **A singular noun should be used for `document` names**

  - ✅ `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/claudio`

- **A plural noun should be used for `collection` annd `store` names**

  - ✅ `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players`

  - ✅ `http://api.music.restapi.org/artists/mikemassedotcom/playlists`

- **A verb or verb phrase should be used for `controller` names**

  - ✅ `http://api.college.restapi.org/students/morgan/register`
  - ✅ `http://api.example.restapi.org/lists/4324/dedupe`

- **Variable path segments may be substituted with identity-based values.**

  - Template: `http://api.soccer.restapi.org/leagues/{leagueId}/teams/{teamId}/players/{playerId}`
  - Example: `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/21`

- **CRUD function names should not be used in URIs**
  - ❌ `http://api.college.restapi.org/students/morgan/delete`

## URI Query Design

- **The query component of a URI may be used to filter `collections` or `stores`.**

  - `http://api.college.restapi.org/users?role=admin`

- **The query component of a URI should be used to paginate collection or store results.**
  - `http://api.college.restapi.org/users?pageSize=25&pageStartIndex=50`

When the complexity of a client’s pagination (or filtering) requirements exceeds the simple formatting capabilities of the query part, consider designing a special controller resource that partners with a `collection` or `store` such as this `http://api.college.restapi.org/users?search` the `search` controller may accept more complex inputs via a request’s entity body instead of the URI’s query part.

# Chapter 03: Interaction Design with HTTP

This chapter discussing request methods and response status codes.

HTTP request line Syntax:

```
Method SP Request-URI SP HTTP-Version CRLF
```

## Request Methods

**GET and POST must not be used to tunnel other request methods**

Means that HTTP's GET and POST methods should not be employed to emulate or mimic other HTTP request methods, such as PUT, DELETE, or PATCH. Each HTTP method has a specific purpose, and using GET and POST to perform actions that should be handled by other methods is not a recommended practice for designing RESTful APIs.

**GET must be used to retrieve a representation of a resource**

Use the `GET` method to retrieve a resource, GET requests may contain Headers but nobody. no constraints preventing you from sending a body in a GET request but it's not preferable as a best practice.

**HEAD should be used to retrieve response headers**

Clients use HEAD to retrieve the headers without a body. In other words, HEAD returns the same response as GET, except that the API returns an empty body.

**PUT must be used to both insert and update a stored resource.**

Must return the new representations of the updated and created resource

**POST must be used to create a new resource and Execute Contollers**

The POST requestt body contains the suggested state representation of the new resource to be added to the server-owned collection.

Also, it's used to invoke `function-oriented` controller resources such as if we have an email confirmation mechanism that sends a confirmation code to the client in email, and we need to create the `resend code` functionality. we must use the POST Method to call the endpoint that resends the confirmation code.

**DELETE must be used to remove a resource from its parent**

After performing the DELETE request the resource must not be available to the client anymore, which means the next `GET/Head` Must return a `404` Status Code.

**OPTIONS should be used to retrieve the resource’s available interactions**

The response body may contain a list of links that describe the available actions and links that can be done with the specified resource.

## Response Status Codes

---

# Refrences

- [O'Reilly REST API Design Rulebook](https://www.oreilly.com/library/view/rest-api-design/9781449317904/)
- [RESTful web API design](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
