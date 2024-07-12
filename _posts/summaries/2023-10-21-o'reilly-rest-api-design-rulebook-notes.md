---
title: "O'Reilly REST API Design Rulebook Notes"
classes: wide
header:
  teaser: /assets/images/summaries/rest-api-design-rulebook.png
ribbon: ForestGreen
description: "A summary of lessons learned from the O'Reilly REST API Design Rulebook and some Spring Boot implementations"
categories:
  - Summaries
tags:
  - RESTful API
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

In the `Data Acquisition and Control` group at the **European organization for Nuclear Research (CERN)**, The web starts with a clever programmer who had a great idea for a new
software project.

![Tim photo](https://cds.cern.ch/images/CERN-GE-9407011-31/file?size=large)

In December 1990, `Tim Berners-Lee` started a non-profit software project called "WorldWideWeb" to facilitate the sharing of knowledge. He worked on this project for a year and he invented and implemented the following:

- The Uniform Resource Identifier (**URI**)
- The HyperText Transfer Protocol (**HTTP**)
- The HyperText Markup Language (**HTML**)
- The first web server
- The first web browser called **WorldWideWeb** then called **Nexus**
- The first **WYSIWYG** HTML editor.

**WYSIWYG** is standing for: What You See Is What You Get.

![the first website on the web](https://cds.cern.ch/images/CERN-HOMEWEB-PHO-2019-004-1/file?size=large)

In August 1991, the web began to grow exponentially, within five years the number of web users skyrocketed to 40 million, The number was doubling every two months. The Web was growing too large, too fast, and it was heading to collapse because the web traffic was outgrowing the capacity of the internet infrastructure.in addition the Web's protocols were not uniformly implemented and they lacked support for caches and other stabilizing intermediaries.

## Web Architecture

In late 1993, Roy Fielding, co-founder of the Apache HTTP Server Project, Became concerned by the web's scalability problem. After analyzing, Fielding recognized that the web's scalability was governed by a set of key constraints. It was a 6 constraints as the following:

- **Client-Server**
- **Uniform Interface**
- **Layered system**
- **Cache**
- **Stateless**
- **Code-on-demand**

### Client-Server

The separation of concerns is the core theme of the Web’s client-server constraints. The Web is a client-server-based system, in which clients and servers have distinct parts to play. They may be implemented and deployed independently, using any language or technology, so long as they conform to the Web’s `uniform interface`.

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

The Web makes heavy use of code-on-demand, a constraint that enables web servers to temporarily transfer executable programs, such as scripts or plug-ins, to clients. like the code that runs on the old Flash Player.

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
  - ✅ `http://localhost:8000/api/v1/accounts/1/orders`
  - ✅ `http://localhost:8000/api/v1/lessons/1/words` such as the following implementation:

  ```java
    @GetMapping("/{id}/words")
    public ResponseEntity<?> goodExample(@PathVariable Integer id){}
  ```

- **A trailing forward slash (/) should not be included in URIs, Examples:**

  - ✅ `http://localhost:8000/api/v1/accounts/1/orders`

  ```java
    @GetMapping("/{id}/words")
    public ResponseEntity<?> goodExample(@PathVariable Integer id){}
  ```

  - ❌ `http://localhost:8000/api/v1/accounts/1/orders/`

  ```java
    @GetMapping("/{id}/words/")
    public ResponseEntity<?> badExample(@PathVariable Integer id){}
  ```

- **Use Hyphens `( - )` instead of Underscores `( _ )` to improve the readability of names in long path segments, Examples:**

  - ✅ `http://api.example.restapi.org/blogs/mark-masse/entries/this-is-my-first-post`

  ```java
    @GetMapping("/{id}/lesson-words")
    public ResponseEntity<?> goodExample(@PathVariable Integer id){}
  ```

  - ❌ `http://api.example.restapi.org/blogs/mark-masse/entries/this_is_my_first_post`

  ```java
    @GetMapping("/{id}/lesson_words")
    public ResponseEntity<?> badExample(@PathVariable Integer id){}
  ```

- **Lowercase letters should be preferred in URI paths**

  | No. | URI                                               | Note                                             |
  | :-: | :------------------------------------------------ | :----------------------------------------------- |
  |  1  | `http://api.example.restapi.org/my-folder/my-doc` | Totally fine                                     |
  |  2  | `HTTP://API.EXAMPLE.RESTAPI.ORG/my-folder/my-doc` | URI number `2` is the same as the URI number `1` |
  |  3  | `http://api.example.restapi.org/My-Folder/my-doc` | This isn't the same as number `1` or `2`         |

- **File extensions should not be included in URIs. Instead of providing resource extension use the `Accept` request header.**

  - ✅ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall`
  - ❌ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall.json`

  ```java
    @GetMapping(path = "/{document}", produces = {MediaType.APPLICATION_JSON_VALUE,
                                                  MediaType.APPLICATION_XML_VALUE})
        public String getData(
            @RequestHeader("Accept") String acceptHeader,
            @PathVariable String document)
        {
            // Client accepts JSON // fetch and return data as json here
            if (acceptHeader.contains(MediaType.APPLICATION_JSON_VALUE)) {}
            // Client accepts XML fetch and return data as xml here
            else if (acceptHeader.contains(MediaType.APPLICATION_XML_VALUE)) {}
            // Client accepts neither JSON nor XML, handle accordingly
            else {}
        }
  ```

## URI Authority Design

**_Consistent subdomain names should be used for your APIs_**

The top-level domain and first subdomain names (e.g., `soccer.restapi.org`) of an API should identify its service owner. The full domain name of an API should add a subdomain named api. For example: `http://api.soccer.restapi.org`

**_Consistent subdomain names should be used for your client developer portal_**

Use `http://developer.soccer.restapi.org` to help onboard new clients with documentation, forums, and self-service provisioning of secure API access keys

## Resource Archetypes

![Resource Archetypes_pages-to-jpg-0001](https://github.com/0xGhazy/0xGhazy/assets/60070427/f61e51d1-2b79-4b1b-812e-891a17b39f8c)

## URI Path Design

- **A singular noun should be used for `document` names**

  - ✅ `http://api.soccer.restapi.org/leagues/seattle/teams/trebuchet/players/claudio`

- **A plural noun should be used for `collection` and `store` names**

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

  ```java
    @GetMapping("")
    public ResponseEntity<?> getAllUsers(
    @RequestParam(value = "pageNo", defaultValue = AppConstants.DEFAULT_PAGE_NUMBER, required = false) int pageNo,
    @RequestParam(value = "pageSize", defaultValue = AppConstants.DEFAULT_PAGE_SIZE, required = false) int pageSize,
    @RequestParam(value = "sortBy", defaultValue = AppConstants.DEFAULT_SORT_BY, required = false) String sortBy,
    @RequestParam(value = "order", defaultValue = AppConstants.DEFAULT_SORT_ORDER, required = false) String order
    ){}
  ```

When the complexity of a client’s pagination (or filtering) requirements exceeds the simple formatting capabilities of the query part, consider designing a special controller resource that partners with a `collection` or `store` such as this `http://api.college.restapi.org/users?search` the `search` controller may accept more complex inputs via a request’s entity body instead of the URI’s query part.

---

# Chapter 03: Interaction Design with HTTP

in this chapter we'll discussing request methods and response status codes. HTTP request line Syntax:

```
Method SP Request-URI SP HTTP-Version CRLF
```

## Request Methods

**GET and POST must not be used to tunnel other request methods**

Means that HTTP's `GET` and `POST` methods should not be employed to emulate or mimic other HTTP request methods, such as PUT, DELETE, or PATCH. Each HTTP method has a specific purpose, and using GET and POST to perform actions that should be handled by other methods is not a recommended practice for designing RESTful APIs.

**GET must be used to retrieve a representation of a resource**

Use the `GET` method to retrieve a resource, GET requests may contain Headers but nobody. _No constraints preventing you from sending a body in a GET request but it's not preferable as a best practice._

**HEAD should be used to retrieve response headers**

Clients use `HEAD` to retrieve the headers without a body. In other words, HEAD returns the same response as GET, except that the API returns an empty body.

**PUT must be used to both insert and update a stored resource.**

Must return the new representations of the updated and created resource

**POST must be used to create a new resource and Execute Controllers**

The `POST` request body contains the suggested state representation of the new resource to be added to the server-owned collection.

Also, it's used to invoke `function-oriented` controller resources such as if we have an email confirmation mechanism that sends a confirmation code to the client in email, and we need to create the **resend code** functionality. we must use the POST Method to call the endpoint that response the confirmation code. Example: `/users/100/resend`

**DELETE must be used to remove a resource from its parent**

After performing the `DELETE` request the resource must not be available to the client anymore, which means the next `GET/Head` Must return a `404` Status Code.

**OPTIONS should be used to retrieve the resource’s available interactions**

Clients may use the `OPTIONS` request method to retrieve resource metadata that includes an `Allow` header value. For example:
`Allow: GET, PUT, DELETE`. The response body may contain a list of links that describe the available actions and links that can be done with the specified resource.

## Response Status Codes

|        Category        | Description                                                                                    |
| :--------------------: | :--------------------------------------------------------------------------------------------- |
| **1xx**: Informational | Communicates transfer protocol-level information.                                              |
|    **2xx**: Success    | Indicates that the client’s request was accepted successfully.                                 |
|  **3xx**: Redirection  | Indicates that the client must take some additional action in order to complete their request. |
| **4xx**: Client Error  | This category of error status codes points the finger at clients.                              |
| **5xx**: Server Error  | The server takes responsibility for these error status codes                                   |

**200 (“OK”) should be used to indicate nonspecific success**
It indicates that the REST API successfully carried out whatever action the client requested, A 200 response should include a response body.

**200 (“OK”) must not be used to communicate errors in the response body**

**201 (“Created”) must be used to indicate successful resource creation**
return the newly created resource in the response body.

**202 (“Accepted”) must be used to indicate successful start of an asynchronous action**
A 202 response indicates that the client’s request will be handled asynchronously. This response status code tells the client that the request appears valid, but it still may have problems once it’s finally processed. it's used with controller resources.

**204 (“No Content”) should be used when the response body is intentionally empty**
The 204 status code is usually sent out in response to a `PUT`, `POST`, or `DELETE` request, when the REST API declines to send back any status message or representation in the response message’s body.

An API may also send 204 in conjunction with a `GET` request to indicate that the requested resource exists, but has _no state representation to include in the body_.

**301 (“Moved Permanently”) should be used to relocate resources**

The 301 status code indicates that the REST API’s resource model has been significantly `redesigned` and a new permanent URI has been assigned to the client’s requested resource. The REST API `should specify` the new URI in the response’s `Location` header.

**302 (“Found”) should not be used**

The intended semantics of the 302 response code have been misunderstood by programmers and incorrectly implemented in programs since version 1.0 of the HTTP
protocol.

The confusion centers on whether it is appropriate for a client to `always automatically issue a follow-up GET request` to the URI in response’s `Location` header, regardless of the original request’s method.

**303 (“See Other”) should be used to refer the client to a different URI**

A 303 response indicates that a controller resource has finished its work, but instead of sending a potentially unwanted response body, it sends the client the URI of a response resource. Then the client can perform GET to the value of the `Location` header.

**304 (“Not Modified”) should be used to preserve bandwidth**

used when there is nothing to send in the body, whereas 304 is used when there is state information associated with a resource but the client already has the most recent version of the representation.

**307 (“Temporary Redirect”) should be used to tell clients to resubmit the request to another URI**

A REST API can use this status code to assign a temporary URI to the client’s requested resource. For example, a 307 response can be used to shift a client request over to another host.

**400 (“Bad Request”) may be used to indicate nonspecific failure**

400 is the generic client-side error status, used when no other 4xx error code is appropriate.

**401 (“Unauthorized”) must be used when there is a problem with the client’s credentials**

**403 (“Forbidden”) should be used to forbid access regardless of authorization state**

A 403 error response indicates that the client’s request is formed correctly, but the REST API refuses to honor it.

**404 (“Not Found”) must be used when a client’s URI cannot be mapped to a resource**

**405 (“Method Not Allowed”) must be used when the HTTP method is not supported**

A 405 response must include the Allow header, which lists the HTTP methods that the resource supports. For example: `Allow: GET, POST`

**406 (“Not Acceptable”) must be used when the requested media type cannot be served**

The 406 error response indicates that the API is not able to generate any of the client’s preferred media types, as indicated by the `Accept` request header.

**409 (“Conflict”) should be used to indicate a violation of resource state**

For example, a REST API may return this response code when a client tries to delete a non-empty store resource.

**412 (“Precondition Failed”) should be used to support conditional operations**

The 412 error response indicates that the client specified one or more preconditions in its request headers, effectively telling the REST API to carry out its request only if certain conditions were met. A 412 response indicates that those conditions were not met, so instead of carrying out the request, the API sends this status code.

**415 (“Unsupported Media Type”) must be used when the media type of a request’s payload cannot be processed**

The 415 error response indicates that the API is not able to process the client’s supplied media type, as indicated by the `Content-Type` request header.

**500 (“Internal Server Error”) should be used to indicate API malfunction**

A 500 error is never the client’s fault and therefore it is reasonable for the client to retry the exact same request that triggered this response, and hope to get a different response.

---

# Refrences

- [O'Reilly REST API Design Rulebook](https://www.oreilly.com/library/view/rest-api-design/9781449317904/)
- [RESTful web API design](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
