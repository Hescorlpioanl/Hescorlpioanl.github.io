---
title: "Clean Code Summary"
classes: wide
header:
  teaser: /assets/images/summaries/clean-code.jpg
ribbon: ForestGreen
description: "Clean Code book summary"
categories:
  - Summaries
tags:
  - clean-code
toc: true
---

<div align="center">

بسم الله الرحمن الرحيم <br> احييكم بتحية الإسلام السلام عليكم ورحمة الله تعالى وبركاته

</div>

# Chapter 01: Meaningful Names

## Intention-Revealing Names

Use clear names that reveal your intention and what you think, Don't use `x` or any single-letter character, Instead use a name that represents the variable value.

```java
class ClassName{
	// ❌ Bad example of variable naming
	private int d;

	// ✅ Good example of variable naming
	private int elapsedTimeInDays;
}
```

## Avoid Disinformation

Do not refer to a grouping of accounts as an `accounts` unless it’s a List because The word `list` means something specific to programmers. So you can use `accounts` instead and it will be better.

## Make Meaningful Distinctions

Don't use `a1`, and `a2`, Using the number series is unsuitable for intentional naming.

```java
public static void copyChars(char a1[], char a2[]) {
	for (int i = 0; i < a1.length; i++) {
		a2[i] = a1[i];
	}
}
```

Using `source` and `destination` as parameter names is better than `a1`, and `a2`.
Don't use noisy words too such as `variable`, `table`, `class`, `string`, and `object`. The `variable` word shouldn't appear in the variable name, and also `table` in the table name, and so on.

```java
// ❌ Bad example
String nameString = "hossam";
String nameVariable = "hossam";

// ✅ Good example
String name = "hossam";
```

The variable `name` will always contain a name, It'll not contain numbers or another object so we didn't need to add `String` after the variable name.

## Use Searchable Names

Here in my personal opinion, this is an extension for using meaningful names, if I have a task to change some code related to account balance, I'll search first where does `balance`, and `account` words appear and my task will be easy. On the other hand, if we are using `b` for balance and `ac` for account, It will be hard to search for words that we might except it's is exist to help us do our job.

You can use one letter variable name in case of small scopes such as for loop you can use `i`, and `j`. Because the loop scope is very limited and function is very short we can see what is `i` and where is being used.

## Avoid Encoding

Here the word `encoding` means the usage of pre/post-fix words in our variable names.

### Member Prefixes

In the class context, we can use `name`, `phone`, and `address` for a data member's variable names, without prefixing them with any kind of symbols or chars. such as `m` for data members, or `p` for pointers, and so on.

### Interfaces and Implementations

Here in the `Java` context, it's preferred to name your interface with its name without any encoding and encode the classes that implement it. For example, we have an interface called `AccountRepository` Here we have no encoding, For the classes that implement this interface we can use the `Impl` post-fix for their names such as `AccountRepositoryImpl`.

For the `C#` Guys they may prefer the `I` for the implementation so they will call it `IAccountRepository`. This is good too as long as we follow the community convention.

## Avoid Mental Mapping

Mind mapping is the effort you have to put in to understand how the code works. It would be helpful to avoid unnecessary burdens on any other developer who will be reading your code and trying to understand what is happening here.

So don't use `d` for `dateOfBirth`, what is preventing you from making it `bod`, or `dateOfBirth`? One letter variables add additional effort to be make to understand and search in your code what does letter `d` refers to. Using one letter such as `i`, `j`, and `k` in looping is OK because the loop scope is very small and the `i` will not be used any where outside the loop body, Also it's good to use descriptive name in the loops such as if you are lopping in accounts list `ArrayList<Accounts>` it is good to name your iterator variable to `account`.

Here We can add some rules to follow that help us to avoid mental mapping.

### Avoid magic number

```java
// ❌ Bad example
// what does 3000 refers to?
if (timeDuration > 3000)
{
	// code
}

// ✅ Good example
int readTimeout = 3000;
if (timeDuration > readTimeout)
{
	// code
}
```

### Explained code

```java
// ❌ Bad: Complex logic with unclear purpose
// What do all of these vars stand for?
if (a == b && c != d || e == f) {
	// some action
}

// ✅ Good example
boolean isSameCategory = a == b;
boolean isDifferentType = c != d;
boolean isSpecialCase = e == f;
if ((isSameCategory && isDifferentType) || isSpecialCase) {
	// Here if you even have some bad variable names now you know what (a == b) mean. and so on.
	// ChatGPT helps me to get this example <3
}
```

## Class Names

For class names, you should make their noun, or noun phrase names such as `Customer`, `WikiPage`, `Account`, etc...
You should also avoid words like `Manager`, `Processor`, `Data`, and `Info` in the name of the class because it's too general and doesn't specify what does this class do?

For example, what if I have a class called `AccountManager`, Can you tell me what kind of management this class handles?
Does it do creation and initialization? or does business logic? or dealing with account representation?

On the Other hand, we can have the `Account` class that represents an account in my system, `AccountService` that handles the business logic, and also the `AccountRepository` that handles the account persistence in the database. and so on.

## Method Names

Method names should have a verb in its name. Such as `setX`, `getX`, `isX`, `postPayment` , etc...

When constructor is overloaded use static factory methods with the name that describe the arguments.

Example:

```java
@Data
@ToString
public class Car {
    private String make;
    private String model;
    private int year;
    private String fuelType; // Optional field with default value

    // Constructor with make, model, and year
    private Car(String make, String model, int year) {
        this(make, model, year, "Gasoline");
    }

    // Constructor with make, model, year, and fuel type
    private Car(String make, String model, int year, String fuelType) {
        this.make = make;
        this.model = model;
        this.year = year;
        this.fuelType = fuelType;
    }

    // Static factory methods
    public static Car createWithDefaultFuel(String make, String model, int year) {
        return new Car(make, model, year);
    }

    public static Car createWithCustomFuel(String make, String model, int year, String fuelType) {
        return new Car(make, model, year, fuelType);
    }
}
```

## Don’t Be Cute - ما تحلوش في الكود

If names are too clever, they will be memorable only to people who share the author's sense of humor. Here is an example will explain what it means.

```java
import java.io.*;
import java.net.*;

public class SimpleTCPServer {
    private ServerSocket serverSocket;
    private Socket clientSocket;
    private PrintWriter out;
    private BufferedReader in;

    public void start(int port) {
        try {
	        // open connection here and accept incoming requests
        } catch (IOException e) {
            // handle exception
        } finally {
            stop();
        }
    }

	// ✅ Good example of close method
    public void stop() {
        try {
            // terminate all connections
            // and close the server
        } catch (IOException e) {
            // handle exception
        }
    }

	// ❌ Good example of close method
    public void qatr6ElaTelt() {
        try {
            // terminate all connections
            // and close the server
        } catch (IOException e) {
            // handle exception
        }
    }
}
```

<img align="right" 
     height="200"
     width="200"
     src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTKuCFMiw5sQfH7KmjoSlBWnR2qQCUdkH3wVQ&s"
     alt="Prof.-Adel-Shakal"/>

In the TCP server above we have `start`, and `stop` methods, if i hide the methods implementation you will still get what does this methods do, `start` for starting the TCP server to receive and accept connections requests, and `stop` for closing the server and terminate all open connections.

**What about `qatr6ElaTelt` ? What does it do? What does it mean?**

For me I know what does it mean by reading the name only without even see the implementation. Because i know the `meme`, `Joke`, or the `reference` behind it. Not all people know **Prof. Adel Shakal**, So it will so hard for them to expect what does this method do without reading the implementation.

## Pick One Word per Concept

Here we should use one word for one concept during the project or code base, Choose one of `get`, `fetch`, or `retrieve` to describe the action of getting data from other sources for example. Don't try to mix them in the same code base! Can you tell me what is the difference between `getUser()`, and `fetchUser()` in the same code base? or another example is the difference between `UserController`, `UserRequestHandler`, and `UserHttpHandler` in the same project. It's all about using a uniform name schema for a code base.

## Don’t Pun

Here it's all about **avoiding** multiple meanings for one word and striving for a single responsibility for the terms. Each term in your code should have a single, clear responsibility. Reusing terms in different contexts can make the code harder to understand and maintain.

**Bad example**:

```java
public class AccountManager {
    public void add(Account account) {
        // Adds an account to the system
    }

    public int add(int a, int b) {
        // Adds two numbers
        return a + b;
    }
}
```

Here the term `add` is used to describe adding the account to the database for example, and adding two numbers. which could lead to confusion about using one word for two different meanings.

**Good Example**:

```java
public class AccountManager {
    public void addAccount(Account account) {
        // Adds an account to the system
    }

    public int addNumbers(int a, int b) {
        // Adds two numbers
        return a + b;
    }
}
```

Using `addAccount` is now different from `addNumbers` even if they start with the same prefix, they give a different meaning for readers.

## Use (Solution & Problem) Domain Names

The readers of your code are developers too, so use "solution" domain names in your code such as technical terms, algorithm names, and pattern names. It will be easy for other developers to understand and you won't have to explain it in your code.

Also use Problem domain names that reflect the terminology and concepts of the problem domain or the business logic of your application. For example if you are working on an E-Commerce application you should have entities that represent the problem domain components like `order`, `cart`, and `item`, etc..

## Add Meaningful Context

**Meaningful Context** means that names of variables, methods, classes, and other code elements should be descriptive and convey their purpose or role within the code.

```java
// Less meaningful
double temp;

// More meaningful
double temperatureInCelsius;
```

```java
// Less meaningful
String firstname; // firstname of what?
String lastname;  // lastname of what?

// More meaningful
public class Person {
	private String firstname;
	private String lastname;
}
```

## Don't Add Gratuitous Context

Avoid using unnecessary or redundant prefixes and suffixes in names. Names should be clear and concise, reflecting the actual role or type without extra context that doesn’t add value. Overly complex or verbose names can make code harder to read and understand.

For example, if we have a project called **`Gas Station Deluxe`**, it is a bad idea to prefix all project source code files with `GSD`. It will make the code completion after writing the `G` letter useless because it will suggest all your project files. You just add unnecessary 3 letters or more to every class name.

_Shorter names are generally better that longer ones, as long as they are clear._

---

<div class="fb-comments" data-href="https://developers.facebook.com/docs/plugins/comments#configurator" data-width="" data-numposts="10"></div>
