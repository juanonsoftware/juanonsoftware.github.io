---
layout: post
title: About Rabbit.Foundation Package
tags: [projects]
---

The [Rabbit.Foundation][1] package is design to provide additional neccessary utilities for your application

##1. DataItem

##2. SmartList

##3. Extension methods

###1. String extension methods
**GetSubstring**

Get a substring relatively from a source. It tries to return a meaningful substring. Example:

```
var source = "This is a source string";
var substring = source.GetSubstring(6);
```

And the result will be "This is" (length = 7)

We can also get a substring absolutely using another overload

```
var source = "This is a source string";
var substring = source.GetSubstring(6, String.Empty);
```
And the result will be "This i" (length = 6)

###2. Stream extension methods

**ReadAllTextUnicode**

Read the stream and return all unicode characters inside

**ReadAllText**
Read the stream and return all characters inside by specified encoding

###3. XML extension methods

**ToUnicodeText** and **ToText**

Return text representation of an XML node. If the node is XmlDocument, the result contains xml declaration tag.

[1]: https://www.nuget.org/packages/Rabbit.Foundation/
