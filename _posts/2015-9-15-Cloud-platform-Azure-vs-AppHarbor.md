---
layout: post
title: Cloud platform Azure / AppHarbor - The differences
tags: [cloud]
---

I have deployed application to Microsoft Azure and AppHabor but there are differences bitween two platform
from the model to coding. It took me times, and I noted there for references.

##1. The model differences

**Azure** provides free account

- In trial period of 30 days

- We can try all its services with 200$ added to free trial account

- An credit card is required on registration

- Its pricing calculation will be based on your usage

**AppHarbor** provides free account

- With no limit on date

- Other services will cost such as database, pricing is based on capacity of your choice

- If adding a custom domain, it cost 10$ / month (quite big?)

##2. Coding

**Azure**

- Configuration variable is very easy, just added on Azure portal and it will override values from Web.config

**AppHarbor**

- Provides Configuration variables. It equals to AppSettings when you get from Web.config file

## References

[1]: https://msdn.microsoft.com/en-us/data/jj591617.aspx
[2]: http://www.codeproject.com/Tips/661053/Entity-Framework-Code-First-Map
[3]: http://mehdi.me/ambient-dbcontext-in-ef6/