---
layout: post
title: Woring with WCF Data Service
tags: [studying, ef]
---

This post will drive you to create WCF Data Service and how to use it in your client application.
A Windows Console application will be created to comsume the service by using its proxy.
For Web application, it can be called by activating URL with HTTP method.


##Creating a WCF Data Service

The WCF Data Service requires us to have a entity model, actually it is a DbContext class.

![_config.yml]([3])

After updated, it looks like below (in my case the class is `TestingModelContainer`)

![_config.yml]({{ site.baseurl }}/images/posts/data-service-code.png)

Now viewing the service on a browser it looks like this, default format applied is Atom

![_config.yml]({{ site.baseurl }}/images/posts/data-service-run.png)

##Consuming the service using OData syntax

Suppose your database has below data and you also use FF browser

![_config.yml]({{ site.baseurl }}/images/posts/data-service-db.png)

### List all categories

![_config.yml]({{ site.baseurl }}/images/posts/data-service-categories.png)

### Filtering data

This case I want to list all categories with Id > 5

![_config.yml]({{ site.baseurl }}/images/posts/data-service-filter.png)


			
References

- [A Beginner's Tutorial for Creating WCF Data Services][1]

- [OData Basic Tutorial][4]

- [Querying the Data Service (WCF Data Services)][5]


[1]: http://www.codeproject.com/Articles/572417/AplusBeginner-splusTutorialplusforplusCreatingpl
[2]: http://stackoverflow.com/questions/16240217/wcf-service-returning-404-on-method-reqests
[3]: http://www.codeproject.com/KB/WCF/572417/codetemplate.jpg
[4]: http://www.odata.org/getting-started/basic-tutorial/
[5]: https://msdn.microsoft.com/en-us/library/dd673933(v=vs.110).aspx