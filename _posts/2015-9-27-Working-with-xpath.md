---
layout: post
title: Working with XPath to query Xml document
tags: [studying]
---

Using XPath is an effecient way to query, filter in XML document. I will explain some practices using XPath.

Related post: [Xml document processing][{{ site.baseurl }}/Xml-processing]

##Prepare a document

Suppose that the XML document will look like (simplified)

```
<Customers>
	<Customer FirstName="Juan" LastName="Ho"></Customer>
	<Customer FirstName="Tuân" LastName="Ha"></Customer>
</Customers>
```

##Code to produce the document

```
var document = new XmlDocument();
var customers = document.CreateElement("Customers");

var customer1 = document.CreateElement("Customer");
customer1.SetAttribute("FirstName", "Juan");
customer1.SetAttribute("LastName", "Ho");

var customer2 = document.CreateElement("Customer");
customer2.SetAttribute("FirstName", "Tuân");
customer2.SetAttribute("LastName", "Ha");

customers.AppendChild(customer1);
customers.AppendChild(customer2);
document.AppendChild(customers);
```

###Query the document using XPath

So the above code produce a simple document with **no custom namespace**.

How to search for the Customer with LastName is *Ha* ? Below is an example

```
// Build a XPath query
const string query = "//Customers/Customer[@LastName='Ha']";
var nodes = document.SelectNodes(query);

// We must do a check because result of the SelectNodes could be null
if (nodes != null)
{
	foreach (XmlNode node in nodes)
	{
		// We must do a check because Attributes could be null
		if (node.Attributes == null)
		{
			continue;
		}

		var firstName = node.Attributes["FirstName"].Value;
		var lastName = node.Attributes["LastName"].Value;
		Console.WriteLine("FirstName: {0}, LastName: {1}", firstName, lastName);
	}
}
```

Another method to retrive nodes is using XPathNavigator. From a XML document we can get navigator.

```
// Create navigator of the document
var navigator = document.CreateNavigator();
var iterator = navigator.Select(query);

Console.WriteLine("Results: " + iterator.Count);

while (iterator.MoveNext())
{
	var firstName = iterator.Current.GetAttribute("FirstName", string.Empty);
	var lastName = iterator.Current.GetAttribute("LastName", string.Empty);
	Console.WriteLine("FirstName: {0}, LastName: {1}", firstName, lastName);
}
```

Two methods produce the same result
`"FirstName: Tuân, LastName: Ha"`

### Querying XML document with custom namespaces

Suppose that we have below document, just a little different on namespace with prefix is **rb**

```
<?xml version="1.0" encoding="utf-8"?>
<rb:Customers xmlns:rb="http://rabbit/xml">
	<Customer rb:First_x0020_Name="Juan" LastName="Ho" />
	<Customer FirstName="Tuân" LastName="Ha" />
</rb:Customers>
```

The XPath's query must be updated to have namespace prefix **rb** and use another version of SelectNodes with
a namespace resolver

```
var resolver = new XmlNamespaceManager(document.NameTable);
resolver.AddNamespace("rb", "http://rabbit/xml");

const string query = "//rb:Customers/Customer[@LastName='Ha']";
var nodes = document.SelectNodes(query, resolver);
```

Similarly, querying through XPathNavigator should be updated as well

```
var navigator = document.CreateNavigator();
var iterator = navigator.Select(query, resolver);
```

References
- XPathNavigator.Select Method [1][1], [2][2]

[1]: https://msdn.microsoft.com/en-us/library/0ea193ac(v=vs.110).aspx
[2]: https://msdn.microsoft.com/en-us/library/6k4x060d(v=vs.110).aspx