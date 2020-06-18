# API-Testing-Best-Practices
The API Testing Best Practices List
API Testing Best Practices
==========================

Welcome! Read This First

**What?**

This repository is a summary and curation of everything you need to know to write and perform effective API or service layer tests.

**Who?**

It is designed to suit developers and testers of all levels. The content is written in an easy-to-grasp manner suitable for beginner developers, while providing the most value to the reader regardless of their expertise.

**When?**

After laying a solid foundation in your application development process through comprehensive unit tests, you need to integrate all component and services. Use this repository as the fundamental guide for your API tests and service-level tests.

## Table of Contents

- [1. What is an API?](#1-what-is-an-api)
- [2. Overview of REST APIs](#2-overview-of-rest-apis)
- [3. HTTP Methods used in APIs](#3-http-methods-used-in-apis)
- [4. When should I use API tests?](#4-when-should-i-use-api-tests)
- [5. How are APIs monitored?](#5-how-are-apis-monitored)
  * [5.1 Request Headers](#51-request-headers)
  * [5.2 Authentication](#52-authentication)
- [6. Best practices](#6-best-practices)
  * [6.1 Choose a reliable API testing tool](#61-choose-a-reliable-api-testing-tool)
  * [6.2 Write clear tests that allow debugging](#62-write-clear-tests-that-allow-debugging)
  * [6.3 Start with API smoke testing](#63-start-with-api-smoke-testing)
  * [6.4 Test the performance of your API continuously](#64-test-the-performance-of-your-api-continuously)
  * [6.5 Simulate production conditions](#65-simulate-production-conditions)
  * [6.6 Take care of your testing parameters](#66-take-care-of-your-testing-parameters)
  * [6.7 API Mocking and Virtualization](#67-api-mocking-and-virtualization)
  * [6.8 Remove dependencies and fixed data sources whenever possible](#68-remove-dependencies-and-fixed-data-sources-whenever-possible)
  * [6.9 Include negative tests in your test suite](#69-include-negative-tests-in-your-test-suite)
  * [6.10 Track all API responses](#610-track-all-api-responses)
  * [6.11 Content extraction](#611-content-extraction)
  * [6.12 Don't neglect security tests](#612-do-not-neglect-security-tests)
  * [6.13 Enforce Service Level Agreements  (SLA)](#613-enforce-service-level-agreements)

## 1. What is an API?

An [application programming interface](https://en.wikipedia.org/wiki/Application_programming_interface)  (API) is a set of protocols that allow application software and their services to communicate with each other. APIs are essential when building and integrating software services because they allow the application to interact with external services. A good example is the widely used [Google Maps API](https://developers.google.com/maps/documentation).

By allowing users to integrate new components and functionalities into existing architecture, APIs provide increased flexibility, simplicity, and opportunities for innovation when building applications.

APIs also allow IT teams to collaborate and speed up the development and deployment process. For this reason, they have increasingly become the epicenter of today's cloud-native application development.

## 2. Overview of REST APIs

The REST  (Representational State Transfer) API design is a set of architectural constraints that every RESTful API must adhere to. RESTful APIs differ from SOAP APIs in that SOAP is a protocol while REST is simply an architectural style.

As defined by Roy Fielding in his doctorate dissertation titled  '[Architectural](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) [Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)', RESTful APIs must comply with the following six architectural principles:

-   **Based on the client-server architecture**--- A RESTful API should have an independent client and server. Their functions should be clearly delineated in that the UI and request-gathering are within the client's domain, while data access, security, and workload management are within the server's domain. All requests should be handled through HTTP

-   **Stateless operation**s --- All operations between the client and server should be stateless. At no point should the client content be stored on the server. Additionally, all session or state management information  is held within the client-side.

-   **Cacheability** -- All resources should enable caching to minimize client-server interactions. The only exception is when caching is explicitly not possible.

-   **Layered system** -- Client-server interaction should work on an architecture with multiple server layers. This enables features like shared caches, load balancing, and security control.

-   **Uniform interface**--- Arguably the core of REST API design, this guideline defines four constraint facets. Resources should be identifiable through a single unique URL and should allow modification or manipulation through representations. Additionally, HTTP messages should be self-descriptive, and hypermedia should serve as the engine of application states.

-   **Code on demand** **---** Servers often send back static information in the form of JSON or XML. However, they should extend their functionality to include the ability to share executable code to the client when necessary.

## 3. HTTP Methods used in APIs

As you interact with the API, HTTP uses different methods to send requests and fetch data. Methods act as request  "verbs" because they tell the server what to do.

Here's a peek into the most common HTTP methods used in APIs.

Note that the requests used are just examples. You should replace mywebsite.com with your domain name and provide a valid path as per your website/application directory.

-   **GET** -- Asks the server to retrieve information or a resource, without modifying it anyway. An example request would be HTTP GET <http://www.mywebsite.com/users>

-   **POST**--- Used for creating a new subordinate resource in a collection of resources. For example, HTTP POST <http://www.mywebsite.com/users/155/accounts>

-   **PUT**  --- PUT requests asks the server to update or replace an existing resource. Unlike posts requests which are made on a resource collection, a PUT request is made on the resource. For instance, HTTP PUT <http://www.mywebsite.com/users/155/>

-   **DELETE**  --- It is used for deleting resources specified in a URL. For example, HTTP DELETE <http://www.mywebsite.com/users/155>

-   **PATCH**--- It is used for performing partial updates or modification of resources, but does not serve as a replacement for the PUT and POST methods. PATCH should only be used when updating an existing resource partially.

Note that HTTP methods are case sensitive and should always be put in uppercase.

When working with APIs, it is important to test whether your HTTP requests are working as desired in the API call.

## 4. When should I use API tests?

An API is a critical application component.  If it breaks or gets compromised, it puts the entire chain of processes built around it at risk. As such, end-to-end API tests should be done after performing comprehensive unit tests covering individual functions.

A good testing practice is to perform fewer tests as you get to higher levels. In line with Mike Cohn's [Test Pyramid concept](https://martinfowler.com/articles/practical-test-pyramid.html), API tests should be done at the service level  (or integration).

![](https://paper-attachments.dropbox.com/s_5F69C49EB0F27E6BA3986898C8AC84EAD54EB4A5AF7BA41D994FB78A5BD18F2F_1591870628660_test+pyramid.png)

Because the API layer connects microservices to each other, or a client to the server, exhaustive API tests should be performed to ensure the following:

-   That the API implementation is working correctly
-   The implementation works as outlined in the requirements specification
-   There are no regressions between code/API merges and releases

API tests aim to optimize performance and secure communication between different services. They provide higher reliability covering interfaces within the application logic but closer to the user  (though not as UI tests).

## 5. How are APIs monitored?

APIs are monitored using several monitoring and testing tools designed to check a number of things including:

-   Performance issues such as high API response time
-   Reliability issues such as difficulty in connecting or getting API responses
-   Structure of response data and its correctness
-   Missing or duplicate functionalities
-   Unused flags
-   Incorrect handling of arguments
-   Failure to handle error conditions
-   Improper warning or errors upon calling an API methods
-   Multi-threading issues
-   Security issues

These are just some of the basic elements included in an API test suite. The goal when testing and monitoring API transactions is to validate every component and process involved in data exchange.

Testing different parameters requires you to configure tests with request headers and authentication methods.

### 5.1 Request Headers
-------------------

When testing the API, it is important to simulate the real-world transaction by including the request headers that make up the API call. In the request header, you can specify whether the HTTP request is a GET or POST, and if it requires any form of authentication or data caching during the session.

Typically, users execute a series of actions that can be viewed as flows. For instance, the sign-up flow, the login flow, or the export data flow.

When testing the API, verify that these HTTP requests execute successfully, and that users receive the correct response.  This can be achieved by examining the request headers, status line, and message body.

An effective approach would be using [Loadmill](https://www.loadmill.io/) to test different methods using different parameters in the API test flow as shown below.

![](https://paper-attachments.dropbox.com/s_5F69C49EB0F27E6BA3986898C8AC84EAD54EB4A5AF7BA41D994FB78A5BD18F2F_1591938846318_LOADMILL+API+TEST.png)

After running the API test, Loadmill returns detailed information showing the results for all HTTP requests in the test case.

![](https://paper-attachments.dropbox.com/s_5F69C49EB0F27E6BA3986898C8AC84EAD54EB4A5AF7BA41D994FB78A5BD18F2F_1591938894268_RESPONSE.png)

A section of the results also shows the HTTP request header:

![](https://paper-attachments.dropbox.com/s_5F69C49EB0F27E6BA3986898C8AC84EAD54EB4A5AF7BA41D994FB78A5BD18F2F_1591938985297_LOADMILL+RESPONSE.png)

### 5.2 Authentication
------------------

Almost every REST API requires users to pass some sort of authentication process. Authentication is when a user or any other entity proves their identity during a connection attempt.

Simulating API transactions in a test would therefore involve testing the supported authentication methods.

The three most common authentication methods used in REST APIs are basic authentication, API keys, and OAuth.

**Basic authentication**

Users send their username or account ID and password in every API call. Here is an example:

``` 
curl "https://samplewesb1.com/" \

     -H "Authorization: Basic dyaXNlcm5hbWU6cGFzc3dvcmQ
```

The string basic shows that we are using the basic access authentication method while the string `*dyaXNlcm5hbWU6cGFzc3dvcmQ=*` represents a base64-encoding of username: password.

**API Keys**

The API service generates a unique key for every account, which users must provide alongside their requests.

The API key is a long, unguessable secret string that the API creates and gives to the user. Its something like:

`t34580942sf4e0b742e66434hasdr0a5ed75f0bjkdbe`

There is no standard [implementation for API keys](https://www.freecodecamp.org/news/best-practices-for-building-api-keys-97c26eabfea9/), but they are often passed alongside the API authorization header.

```
curl "https://example.com/" \

     -H "Authorization: t34580942sf4e0b742e66434hasdr0a5ed75f0bjkdbe
```

**OAuth**

Technically, [OAuth](https://auth0.com/docs/protocols/oauth2) is not only an authentication method but also a verification method.

When a user hits the sign-in button, the system sends an authentication request in the form of an access token. The user then forwards the request to an authentication server, which grants permissions or rejects the request.

The access token is then provided to the user and the API provider and can be checked from time to time. However, the token is only valid for a limited period.

When working with REST APIs, remember that OAuth and Basic authentication methods should never be implemented on plain HTTP. Instead, protect them through SSL/TLS.

## 6. Best practices

This section delves into the best practices every tester should follow to write and perform effective API tests.

### 6.1 Choose a reliable API testing tool
--------------------------------------

Like any other kind of testing, the choice of tool or automation framework used has a substantial impact on the effectiveness and success of your testing efforts. Using a solid API test automation tool brings a wide range of benefits to the testing process, including higher accuracy, reusability, and scalability of tests.

The ideal automation testing tool facilitates smooth measuring, tracking, and testing of the API functionality and performance. Since most allow developers to create complex multi-step testing scenarios, you can easily modify existing test cases or add new tests as the testing requirements change.

This is especially important because, as most applications scale, the scope of testing also increases.

### 6.2 Write clear tests that allow debugging
------------------------------------------

There are a number of reasons that lead to failures when performing API tests, most notably:

-   Flawed test automation
-   Inclusion of race conditions in the test
-   Flawed API functionality
-   Unstable application functionality
-   Implementing changes in the application without modifying the test
-   Environment failure

When writing API tests, give every test case the attention-to-detail it deserves. It is always a good idea to test components separately for all possible configurations.

To simplify the process of debugging, ensure the failure clause of your test is clearly shown in the test report, including detailed descriptive information. This way, it will be easier to run successful test cases, and in case of a failure, you can easily track the underlying cause.

### 6.3 Start with API smoke testing
--------------------------------

Every time you put up a new API for testing, always start by performing a smoke test. This is a quick test done to validate the API code and confirm that the basic and critical functionalities are working.

A typical smoke test calls the API to verify whether it responds. It also calls the REST API using a standard amount of test data, followed by a larger amount of data to check whether it responds correctly.

[Smoke testing](http://softwaretestingfundamentals.com/smoke-testing/) the API also involves verifying whether the API interacts with other application components as well as APIs as desired.

Sanity testing should also be done at the beginning to ensure that the results of your smoke tests make sense in the context of the designated API function or purpose. In a sanity test, you verify whether the API interprets and displays the correct data as expected.

[Smoke testing and sanity testing](https://www.guru99.com/smoke-sanity-testing.html) aims to identify major flaws in a build and provide an immediate resolution before performing the full test. While serving as a great starting point of the testing process, these tests help reduce the amount of time spent in API testing.

### 6.4 Test the performance of your API continuously 
--------------------------------------------------

Testing the availability of an API is not enough. You need to test the performance and ensure it can support the desired load without outages or degradation of service. Ideally, you should check:

-   The number of concurrent server connections it can take before failure
-   Concurrent user load in batches of 10, 50, 100, 200, 500, and so on.
-   The expected throughput and response time for different user loads.
-   The maximum queries or number of calls executed per second.

Continuously testing the API helps you identify performance issues to ensure there are no failures, or speed issues when the application is released to end-users.

### 6.5 Simulate production conditions
----------------------------------

When testing an API, it is important to simulate the conditions it will encounter when released for public use. By doing so, you ensure that the test results reflect the accurate  (or near-accurate) functionality and performance of the API in is intended production environment.

Treating the API like it is in the consumer environment also gives a clue on performance issues that the development team needs to resolve before moving forward. After all, most things tend to break in unpredictable ways when released to a live environment.

### 6.6 Take care of your testing parameters 
-----------------------------------------

An API consists of multiple methods and operations, which should be tested individually and collectively by combining multiple API calls. For comprehensive testing, it is always a good idea to list all the mandatory parameters in your API call/request.

This ensures you do not miss out on some critical parameters required in the JSON response. Additionally, perform syntax tests on individual methods or operations to ensure they align with the parameters defined.

### 6.7 API Mocking and Virtualization
----------------------------------

API testing usually involves testing many other interconnected components in an application. However, there are times that some components required for complete API testing may not be available dues to some unavoidable reasons.

For instance, the component may not be developed yet. Another scenario is when the component holds unusable or insufficient test data. Alternatively, the component might be ready, but it is shared with other teams, so testers cannot use it freely.

When you encounter any of the above scenarios during testing, virtualization is the most viable solution to enable continuous testing of the API as planned.

Virtualization in API testing takes various forms, including:

-   [Virtualization](https://en.wikipedia.org/wiki/Service_virtualization) --- It involves simulating the behavior of complex application components such as database connectivity at the back-end or transport layer protocols  (but not HTTP).

-   [Stubbing](https://en.wikipedia.org/wiki/Method_stub#:~:text=A%20method%20stub%20or%20simply,to%2Dbe%2Ddeveloped%20code.) --- Stubbing is used for creating an emulation of a SOAP or REST web service API.

-   [Mocking](https://en.wikipedia.org/wiki/Mock_object) --- It involves creating mock code objects with the help of frameworks such as Mockito.

### 6.8 Remove dependencies and fixed data sources whenever possible
----------------------------------------------------------------

API endpoints in a live environment usually rely on internal application components and external elements such as other APIs, third-party services, servers, legacy systems, and so on. When performing API tests, you'll also have to deal with input from these dependencies.

The best approach is usually eliminating these dependencies and fixed data sources whenever possible. When you test some API's with static data collected from other sources, you're not testing the API holistically.

Instead, you should do away with these dependencies and follow actual user flows. This makes your API tests complete and more efficient.

### 6.9 Include negative tests in your test suite
---------------------------------------------

It is normal to perform positive tests in any scenario. For instance, inputting valid data and verifying whether an API request is processed correctly. Although this is the standard way of testing, it is important to verify the results with negative tests.

Performing negative tests with equal due diligence makes it easier for testers to identify whether the API deals with receiving invalid or incorrect data gracefully.

The expected response in such a negative test is that the API should provide an error message, but not encounter a hard crash or complete stop.

Negative tests help verify the completeness of the application as well as its ability to accommodate user error.

### 6.10 Track all API responses
----------------------------

API responses are some of the most critical elements that you should track and record during the development process. However, the vast majority of developers and testers tend to discard these responses.

It is vital to track API responses because they provide a benchmark of how it worked during a particular build at the time of testing.

Responses are preserved so that, when the API undergoes a series of changes, it will be easier to examine responses from preceding builds in future. When an error occurs, it will be easier to figure out the modification leading to an error.

### 6.11 Content extraction
-----------------------

Content extraction is the process of parsing and extracting content from the response data. Extraction is particularly helpful when you need to generate dynamic content for subsequent requests, for instance, cookie values or access tokens.

When extracting content from HTTP responses, you must set the extraction feature the request options parameter. The steps involved are:

-   State the desired type. This could be xpath, jsonpath, header, etc.
-   Specify the reference names.
-   Specify the expressions.

Note that you can use multiple content extractions in a single request.

Here is an example:


```
session.get("/tokens", {
  extraction: {
    jsonpath: {
      "accessToken": "authorization.token",
      "checksum": "authorization.checksum"
    }
  }
});
```

The getVar() function can help us use extracted content in subsequent requests:


```
session.get("/ping?token=" + session.getVar("accessToken"));
```

Here is another example extraction using Xpath, a method to extract information from an XML document.

Given the response and request options below:


```
//This is the Response

<?xml version="1.0" encoding="UTF-8"?>
<users>

<user>
<firstname>Martin</firstname>
<lastname>Luther King</lastname>
<email>martinLking@sampleweb1.com</email>
</user>
</users>

//Request Options 

session.get("/tokens", {
extraction: {
xpath: {
"email": "/users/user[1]/email"
}
}
}); 
```

This makes the email a dynamic data source available within the same session.


```
session.put("/user?email=" + session.getVar("email"));
```

### 6.12 Do not neglect security tests
---------------------------------

Cyber threats such as data breaches are increasing at an exponential rate in today's highly interconnected world. Every year, thousands of organizations and businesses say their applications  (and application data) have been compromised.

A poorly implemented API is one of the most common loopholes that attackers can exploit.

Part of the API testing process should, therefore, focus on testing the security features of the API. When testing, ensure you verify that the API authentication works effectively and that data exchange between components is secure.

API methods, if not implemented correctly, also allow third parties to compromise the application. You should, therefore, test all methods to ensure they do not provide a loophole for compromise when making an API call.

### 6.13 Enforce Service Level Agreements
--------------------------------------------

The API testing process should also focus on proper enforcement of service-level-agreements (SLAs), especially when the API is fully functional and awaits release. This helps testers check the performance issues underlying an API.

The quicker you identify these issues, the easier it will be to fix them and avoid breaching SLAs when the application is released.

API testing is an integral element of the application development, and should always be done holistically to ensure a polished final product. For the best results, keep the above testing practices in mind.
