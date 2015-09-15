---
layout: post
title: DDD - Coding for Domain-Driven Design
tags: [ef]
---

Reading a series of posts [Data Points - Coding for Domain-Driven Design: Tips for Data-Focused Devs][1]
have a look back to what I have done with.

** Query on Repository
This point, we shouldn't have all navigation properties from an aggregate object, example

Order <--> LineItem relationship is 1:0...* but LineItem should not contain Order definition.

** Value objects

In EntityFramework, there is a ComplexType attribute to specify a type is Value Object, similar to
Component mapping in NHibernate.

An example is:

*Customer has BindingAddress and ShippingAddress* properties.

If they are ComplexType, so Customer has no navigation property to Address type, instead all fields of
Address will appear on Customer. From database normalization point of view, is it still good ?
I will have to dig into this question...

[1]: https://msdn.microsoft.com/magazine/dn342868.aspx