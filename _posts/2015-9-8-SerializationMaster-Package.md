---
layout: post
title: SerializationMaster - Work with serialization easier
tags: [projects]
---

This package has created to isolate serialization progress from depending on any serialization implementation.
It provides extension methods to serialize / de-serialize any object at any layer of your application.
You only need to configure one time at application entry point.

## Installation

The easiest way to reference this package is using Package Manager Console, using this command:

`Install-Package Rabbit.SerializationMaster`.

Here is [NuGet Package] location[1].

## Build-in functions

**Serialize**

To serialize an object to string, just do it as below after added reference `Rabbit.SerializationMaster`

```
var result = source.Serialize();
```

**Deserialize**

To de-serialize a string back to object, just do it as below after added reference `Rabbit.SerializationMaster`

```
var entity = result.Deserialize<DataItem>();
```

**Configuration**

To make above functions working, at application entry point you must do a little configuration such as:

```
SerializationContext.Current.Initialize(SerializationType.Xml)
```

The *SerializationType* can be one of below values:

1. Use Base64 strategy

This way your object will be converted to base64string. The type of your object must be decorated with a [Serializable attribute][4].

Example this Address type

```
    [Serializable]
    public class Address
    {
        public string Street { get; set; }
        public int Number { get; set; }
    }
```

2. Use DataContractJson strategy

The result string will have json format. This strategy internally wraps [DataContractJsonSerializer][2] class to serialize/deserialize your object.

3. Use Xml strategy

The result string will have xml format. This strategy internally wraps [XmlSerializer][3] class to serialize/deserialize your object.

## Source code

If you are interested in its source, please take a look on [GitHub repository][5]


[1]: https://www.nuget.org/packages/Rabbit.SerializationMaster/
[2]: https://msdn.microsoft.com/en-us/library/system.runtime.serialization.json.datacontractjsonserializer(v=vs.110).aspx
[3]: https://msdn.microsoft.com/en-us/library/system.xml.serialization.xmlserializer(v=vs.110).aspx
[4]: https://msdn.microsoft.com/en-us/library/system.serializableattribute(v=vs.110).aspx
[5]: https://github.com/netvietdev/serialization-master