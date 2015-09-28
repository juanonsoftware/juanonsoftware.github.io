---
layout: post
title: Xml document processing
tags: [studying]
---

XML is widely used in many applications and if familiar with it will be so helpful in coding activity.
In this post we will go through a common scenario which produces string repsentation of a XmlDocument
then you will see different methods with different behaviors they produce.

##Prepare a document

Suppose that the XML document will look like (simplified)

```
<Customers>
	<Customer First Name="Juan" LastName="Ho"></Customer>
	<!-- I want an attribute of "First Name", with a space-->
	<Customer FirstName="Tuan" LastName="Ho"></Customer>
</Customers>
```

Ideally, attribute's name and element's name shouldn't contain special character such as space, colon...
but it can. Look at code to see how it will be done

##Code to produce the document

```
var document = new XmlDocument();

//var customers = document.CreateElement("rb", "Customers", "http://rabbit/xml");
var customers = document.CreateElement("Customers");

var customer1 = document.CreateElement("Customer");
customer1.SetAttribute(XmlConvert.EncodeName("First Name"), "http://rabbit/xml", "Juan");
customer1.SetAttribute("LastName", "Ho");

var customer2 = document.CreateElement("Customer");
customer2.SetAttribute("FirstName", "Tuan");
customer2.SetAttribute("LastName", "Ho");

customers.AppendChild(customer1);
customers.AppendChild(customer2);
document.AppendChild(customers);
```

The comment line is another overload to define prefix and namespace for the `Customers` element.
If namepsace is needed, it should be declared at document level. This is just an example.

###Simply using [OuterXml][1] Property

This property is great, it returns all string representation of any xml node `XmlNode` including XmlDocument, XmlElement.
But one point is that it doesn't include declaration tag

```
<?xml version="1.0" encoding="utf-8"?>
```

###Getting text representation using XmlTextWriter

```
using (var ms = new MemoryStream())
{
	var writer = new XmlTextWriter(ms, Encoding.Unicode);
	document.WriteTo(writer);
	writer.Flush();

	ms.Position = 0;

	var xml = new StreamReader(ms).ReadToEnd();
	Console.WriteLine(xml);
}
```

This code generates below string

```
<rb:Customers xmlns:rb="http://rabbit/xml">
	<Customer rb:First_x0020_Name="Juan" LastName="Ho" />
	<Customer FirstName="Tuân" LastName="Hò" />
</rb:Customers>
```

It's not a well-formed document because of missing a xml declareation like this

```
<?xml version="1.0" encoding="utf-8"?>
```

###Getting text representation using XmlWriter.Create

This version we do a little change on creating the writer

```
using (var ms = new MemoryStream())
{
	var writer = XmlWriter.Create(ms);
	document.WriteTo(writer);
	writer.Flush();
	
	ms.Position = 0;

	var xml = new StreamReader(ms).ReadToEnd();
	Console.WriteLine(xml);
}
```

It will produce a well-formed document, because now the writer is `XmlWellFormedWriter` which declared as an internal class.

```
<?xml version="1.0" encoding="utf-8"?>
<rb:Customers xmlns:rb="http://rabbit/xml">
	<Customer rb:First_x0020_Name="Juan" LastName="Ho" />
	<Customer FirstName="Tuân" LastName="Hò" />
</rb:Customers>
```

One point to note here is that using `XmlWriter.Create` (XmlWellFormedWriter) always write xml declarative element.
So we should have to use XmlTextWriter when we want to get string representation of a XmlElement.


[1]: https://msdn.microsoft.com/en-us/library/system.xml.xmlnode.outerxml.aspx