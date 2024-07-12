---
title: "Builder Design Pattern"
classes: wide
header:
  teaser: /assets/images/Blogs/Design-Patterns/design-patterns.png
ribbon: ForestGreen
description: "Using Builder design pattern to build a response object for ResponseEntity"
categories:
  - Blog
tags:
  - Design Patterns
toc: false
---

السلام عليكم ورحمة الله وبركاته

## Introduction

Hello world, hossam is here again, in this post i'll show you how i used the builder design pattern to build a response object for ResponseEntity yo use it in my spring boot application and how this object helps me to write readable and organized code.

I was building simple api that perform CRUD operations on the database. in this approach i used yo use the hashmap to create a response object such as the following code:

```java
import java.util.HashMap;

public class Response {

	private static HashMap<String, Object> response = new HashMap<>();

	public static HashMap<String, Object> makeResponse(String msg, Integer code, Object data){
		response.put("code", code);
		response.put("message", msg);
		response.put("data", data);
		return response;
	}

	public int getCode(){
		return (int) response.get("code");
	}

	public static HashMap<String, Object> toData(){
		return response;
	}
}
```

in this approach, I have to provide the `makeResponse()` method with its parameters and it was accepted until I added the pagination and other request parameters to get a bulk of records from the database. for this new feature I need to add some metadata -as I call it- about the total number of records remaining, the last page, what is the page size, and what is the total number of records.

here I need to change the Response object to handle the newly needed parameters `metadata`. but for some other requests let me say the simple request such as Getting a record by ID there is no need to have metadata here. in other words, the metadata here or any future parameters may be optional to have in your response object.

here I'll introduce the builder design pattern. The Builder design pattern is a creational design pattern that is used to construct a complex object step by step. It separates the construction of a complex object from its representation, allowing for more flexible and maintainable code.

## Pros and Cons

**Pros**

- Improve Readability and Code Organization: The Builder pattern breaks down the construction process into a series of clear, well-defined steps. which make the code more readable and understandable.

- Parameter Validation: Builders can perform validation and checks on the input parameters during the construction process. this helps you to be sure that your object is valid and initialized as you expected.

- Fluent Interface: Builders can be designed to support a fluent interface, where method calls can be chained together. This results in code that reads like a series of natural language statements, improving the overall readability of the code.

- Optional Parameters: Builders allow for the creation of objects with optional parameters. Developers can choose to set only the parameters they need, making object creation more flexible and reducing the need for multiple constructors.

- Reusable Builders: Once a builder is defined, it can be reused to create multiple instances of objects with similar configurations, saving development effort and promoting consistency.

**Cons**

- The overall complexity of the code increases since the pattern requires creating multiple new classes.

## Implementation

In my case, I created a new class for handling metadata called `ResponseMetadata` with the following code:

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class ResponseMetadata {
    private Integer pageNo;
    private Integer pageSize;
    private Long totalRecords;
    private Integer totalPages;
    private Boolean isLast;
}
```

it's pretty simple, Then I modified the `Response` class constructor and properties to the following:

```java

public class Response {

    private String message;
    private HttpStatus status;
    private Object data;
    private ResponseMetadata responseMetadata;
    private HashMap<String, Object> jsonResponse = new HashMap<>();


    private Response(ResponseBuilder responseBuilder){
        this.message = responseBuilder._message;
        this.status = responseBuilder._status;
        this.data = responseBuilder._data;
        this.responseMetadata = responseBuilder._metadata;
    }
}
```

data is an `Object` because we don't know its type, sometimes it will be one object, and sometimes will be a list of objects.

Now let's implement the `ResponseBuilder` class inside the `Response` class. The idea here is to build the builder with the same properties as the `Response` class to achieve the mapping in the constructor `Response(ResponseBuilder responseBuilder)`.

```java
    @NoArgsConstructor
    public static class ResponseBuilder {

        private String _message;
        private HttpStatus _status;
        private Object _data;
        private ResponseMetadata _metadata;

    }
```

The next step is to implement each property setter method. for example, I have the `message` property so I need to implement a method to set the message value but the idea is that **any method that set property must return the builder type**. the following example shows how to implement the setter method for message property.

```java
/* Code snippet */
    public ResponseBuilder status(HttpStatus status) {
        this._status = status;
        return this;
    }
/* Code snippet */
```

Do it for all ResponseBuilder properties. After that, we need to implement the final method that actually creates the response object with the provided properties. By convention it's called the `build()` method, The following example shows how to implement it:

```java
    public Response build() {
        return new Response(this);
    }
```

It's actually a method invocation to the Response private constructor, the build method returns a Response object -the final result we need-.

Put it all together with the `Response` class code:

```java
import lombok.NoArgsConstructor;
import org.springframework.http.HttpStatus;

import java.util.HashMap;

public class Response {

    private String message;
    private HttpStatus status;
    private Object data;
    private ResponseMetadata responseMetadata;
    private HashMap<String, Object> jsonResponse = new HashMap<>();


    private Response(ResponseBuilder responseBuilder){
        this.message = responseBuilder._message;
        this.status = responseBuilder._status;
        this.data = responseBuilder._data;
        this.responseMetadata = responseBuilder._metadata;
    }

    public HashMap<String, Object> jsonfy(){
        jsonResponse.put("message", this.message);
        jsonResponse.put("status", this.status);
        jsonResponse.put("data", this.data);
        jsonResponse.put("metadata", this.responseMetadata);
        if(jsonResponse.get("metadata") == null){
            jsonResponse.remove("metadata");
        }
        return jsonResponse;
    }

    public HttpStatus getStatusCode(){
        return status;
    }


    @NoArgsConstructor
    public static class ResponseBuilder {

        private String _message;
        private HttpStatus _status;
        private Object _data;
        private ResponseMetadata _metadata;

        public ResponseBuilder message(String message) {
            this._message = message;
            return this;
        }

        public ResponseBuilder status(HttpStatus status) {
            this._status = status;
            return this;
        }

        public ResponseBuilder data(Object _data) {
            this._data = _data;
            return this;
        }

        public ResponseBuilder metadata(ResponseMetadata metadata) {
            this._metadata = metadata;
            return this;
        }

        public Response build() {
            return new Response(this);
        }
    }
}
```

## Result

In the following example in the `getAllBooks` method I create a ResponseMetadata object and fill its properties. then fill the response builder with the needed data such as status, result, metadata, and message. in the `getBookById` method I have no need to add the metadata to the response because it's not available and the builder pattern gives me the opportunity to add or skip it.

```java
	public Response getAllBooks(int pageNo, int pageSize, String sortBy, String order){
        Sort sort = order.equalsIgnoreCase(Sort.Direction.ASC.name()) ? Sort.by(sortBy).ascending(): Sort.by(sortBy).descending();
        Pageable pageable = PageRequest.of(pageNo, pageSize, sort);
        Page<Book> books = repository.findAll(pageable);
        List<Book> listOfBooks = books.getContent();
        List<BookDto> result = listOfBooks.stream().map(book -> mapToDto(book)).collect(Collectors.toList());

        // Initialized the metadata
        ResponseMetadata metadata = new ResponseMetadata();
        metadata.setIsLast(books.isLast());
        metadata.setPageNo(books.getNumber());
        metadata.setPageSize(books.getSize());
        metadata.setTotalPages(books.getTotalPages());
        metadata.setTotalRecords(books.getTotalElements());
        // creating the Response object.
        Response response = new Response.ResponseBuilder()
                .status(HttpStatus.OK)
                .message("books retrieved successfully")
                .data(result)
                .metadata(metadata)
                .build();
		return response;
	}

    public Response getBookById(Long id){
		Book book = repository.findById(id).orElseThrow(() -> new ResourceNotFoundException("Book", "id", id));
        Response response = new Response.ResponseBuilder()
                .data(mapToDto(book))
                .message("Book retrieved successfully")
                .status(HttpStatus.FOUND)
                .build();
        return response;
	}
```

**Get bulk books (get all books) response**

```json
{
  "metadata": {
    "pageNo": 0,
    "pageSize": 100,
    "totalRecords": 5,
    "totalPages": 1,
    "isLast": true
  },
  "data": [
    {
      "id": 7,
      "isbn": "123",
      "title": "My first Book",
      "publicationYear": 2023,
      "price": 200.0,
      "rating": 3.2,
      "description": "Ay haga",
      "edition": "1st",
      "language": "en",
      "publisher": "Flowtech House",
      "numberOfPages": 350,
      "dateAdded": "2023-09-10T00:00:00.000+00:00"
    },
    {
      "id": 6,
      "isbn": "56",
      "title": "My first Book",
      "publicationYear": 2023,
      "price": 200.0,
      "rating": 3.2,
      "description": "Ay haga",
      "edition": "1st",
      "language": "en",
      "publisher": "Flowtech House",
      "numberOfPages": 350,
      "dateAdded": "2023-09-10T00:00:00.000+00:00"
    },
    {
      "id": 5,
      "isbn": "78",
      "title": "My first Book",
      "publicationYear": 2023,
      "price": 200.0,
      "rating": 3.2,
      "description": "Ay haga",
      "edition": "1st",
      "language": "en",
      "publisher": "Flowtech House",
      "numberOfPages": 350,
      "dateAdded": "2023-09-10T00:00:00.000+00:00"
    },
    {
      "id": 2,
      "isbn": "987654321",
      "title": "My first Book",
      "publicationYear": 2026,
      "price": 200.0,
      "rating": 3.2,
      "description": "Ay haga",
      "edition": "1st",
      "language": "en",
      "publisher": "Flowtech House",
      "numberOfPages": 350,
      "dateAdded": "2023-09-10T00:00:00.000+00:00"
    },
    {
      "id": 1,
      "isbn": "753",
      "title": "My first Book",
      "publicationYear": 2023,
      "price": 200.0,
      "rating": 3.2,
      "description": "Ay haga",
      "edition": "1st",
      "language": "en",
      "publisher": "Flowtech House",
      "numberOfPages": 350,
      "dateAdded": "2023-09-10T00:00:00.000+00:00"
    }
  ],
  "message": "books retrieved successfully",
  "status": "OK"
}
```

**Get record by id response**

```json
{
  "data": {
    "id": 1,
    "isbn": "753",
    "title": "My first Book",
    "publicationYear": 2023,
    "price": 200.0,
    "rating": 3.2,
    "description": "Ay haga",
    "edition": "1st",
    "language": "en",
    "publisher": "Flowtech House",
    "numberOfPages": 350,
    "dateAdded": "2023-09-10T00:00:00.000+00:00"
  },
  "message": "Book retrieved successfully",
  "status": "FOUND"
}
```

hope this post helps you to understand the builder design pattern and how it works.

references:

- [Builder](https://refactoring.guru/design-patterns/builder)
- [Design Patterns: 14- Builder [بالعربي]](https://www.youtube.com/watch?v=51Ap3s__P_Q&list=PLsV97AQt78NTrqUAZM562JbR3ljX19JFR&index=14&ab_channel=PassionateCoders%7C%D9%85%D8%AD%D9%85%D8%AF%D8%A7%D9%84%D9%85%D9%87%D8%AF%D9%8A)
