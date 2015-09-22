---
layout: post
title: How to choose a Data access technology
tags: [studying, ef]
---

There are different data access technologies including ADO.NET, Entity Framework, WCF Data Services... with different benifits.
Below is some key points of those technologies

##Choosing ADO.NET

### Positive points
- It is a consistency technology, has been around longer than other technologies,
even an old application or a new app it can be used to work with almost databases.

- Stable in both terms of evolution and quality. It has new features, enhancements, improvements, but not change seen established.
People can use it from .NET 1.0 to .NET 4.5 and most bugs have been fixed.

- Easy to learn and understand. There are examples showing how to handle any issue, problem.

### Negative points
- Coding by hand sometime may have performance issue depending on how it was created.

- It's hard to meet new requirements of the application

- When changing to different database system, the code will be affected and requires to rewrite.

##Choosing EF

### Positive pionts
- It enables developers to manipulate data as domain-specific objects without regard to the underlying structure of the data store.

- Microsoft has made (and continues to make) a significant investment in the EF,
and it’s hard to imagine any scenario in the future that doesn’t take significant advantage of it.

- There are providers (storage model & mapping) to handle different database system, so the model stay consistent.

- Having tools that support build and maintain data access of your application in a much shorter time than ADO.NET would

- We focus on dealing with business objects of your application and don’t have to spend your time overly concerned about the underlying data store

- It allows for a clear separation of concerns between the conceptual model and the underlying data store

- The data store can be changed (quite dramatically) without having to rewrite core application logic.

- The EF is built to fit right in with WCF, which makes them very complementary technologies

### Negative points

- It's very different than ADO.NET and requires learning effort

##Choosing WCF Data Services

To support OData, JSON, REST

### Positive points

- WCF Data Services are particularly well-suited to applications that are exposed via Service-Oriented Architecture (SOA)
and enable you to build applications that outsiders (and insiders for that matter) can easily consume and work with

- It uses the EF as a foundation, most (but not all) scenarios that were appropriate for one would be appropriate for the other.

- WCF Data Services would be overkill for simple one-user scenarios.

- Because data is exposed, when using OData, resources are addressable via URIs. It can be accessed by everone for different platform like mobile...

- WCF Data Services are accessed over HTTP, and almost everyone is familiar with HTTP these days and has access to it.
Someone can literally query your application and get data back without having to write a single line of code.

- Very powerful queries can be constructed with very simple semantics.

- OData serves in different formats including JSON, Atom, XML so problems with firewalls, security will immediately disappear.

### Negative points

- WCF Data Services would be overkill for simple one-user scenarios
