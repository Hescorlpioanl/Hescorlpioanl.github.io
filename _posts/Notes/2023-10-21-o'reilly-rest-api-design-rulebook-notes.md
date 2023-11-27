---
title: "O'Reilly REST API Design Rulebook Notes"
classes: wide
header:
    teaser: /assets/images/Blogs/Learning-Notes/rest-api-design-rulebook.png
ribbon: ForestGreen
description: "my notes for best practice for designing a web RESTful APIs"
categories:
    - Notes
tags:
    - Backend
    - REST APIs
    - Design
toc: true
toc_sticky: true
---

<div align="center">

بسم الله الرحمن الرحيم <br> احييكم بتحية الإسلام السلام عليكم ورحمة الله تعالى وبركاته

</div>
<br>

# Chapter 01: Introduction

comming soon

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

    | No. | URI | Note |
    |:--: |:--- | :--- |
    |  1  | `http://api.example.restapi.org/my-folder/my-doc` | Totaly fine |
    |  2  | `HTTP://API.EXAMPLE.RESTAPI.ORG/my-folder/my-doc` | URI number `2` is the same as the URI number `1` |
    |  3  | `http://api.example.restapi.org/My-Folder/my-doc` | This isn't the same as number `1` or `2`|

- **File extensions should not be included in URIs. Instead of providing resource extension use the `Accept` request header.**
    - ✅ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall`
    - ❌ `http://api.college.restapi.org/students/3248234/transcripts/2005/fall.json`


## URI Authority Design

- *Consistent subdomain names should be used for your APIs*

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
