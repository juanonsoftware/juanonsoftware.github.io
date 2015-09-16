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

- Other services will cost such as database, pricing is based on capacity of your choice.
So if your application can use some free services such as Redis / RavenDB... it is not blocking you.

- If adding a custom domain, it costs 10$ / month (quite big?)

##2. Coding & Deployment

**Azure**

- Configuration variable is very easy, just added on Azure portal and it will override values from Web.config

- All keys will be hidden by default, only when you click on a specific key to display the value.

**AppHarbor**

- Provides Configuration variables. It equals to AppSettings when you get from Web.config file

- All keys will be shown when you enter this page on the portal, maybe some sensitive data can be caught by camara or any eyes around ;)

![_config.yml]({{ site.baseurl }}/images/posts/cloud/apphb-config-variables.png)

- After the app is connected to source code repository, it is not deploying automatically. You must do a push on the repository and it will work.

## References

[1]: https://msdn.microsoft.com/en-us/data/jj591617.aspx
[2]: http://www.codeproject.com/Tips/661053/Entity-Framework-Code-First-Map
[3]: http://mehdi.me/ambient-dbcontext-in-ef6/
