---
layout: post
title: EntityFramework - migrate to latest version on Azure and local database from code
tags: [ef, cloud]
---

When working with EF Code First approach, we must update system database after modified model entities
and of course, we must create a migration (or many migrations, depending on how are your changes).
Later we can update the database manually or automatically (on the first read/write to the database).

## Upgrade a local database

After the model has been changed according to the requirement, we can build the application without any error
but when run the app it will raise a runtime error as below (great one :))

![_config.yml]({{ site.baseurl }}/images/posts/ionline/ef-codefirst-error1.jpg)

Oh, what was wrong? The system database is not equivalent to model. An update action is needed,
we can do it simply as below:

![_config.yml]({{ site.baseurl }}/images/posts/ionline/add-migration-category.jpg)

![_config.yml]({{ site.baseurl }}/images/posts/ionline/update-database.jpg)

OK, it works as expected now locally. But if the database is on the cloud, how to proceed?

## Upgrade a cloud database

Following [this guideline ][1] on asp.net site (section **Deploy to Azure**),
we do a manual update step. It requires VS version >= 2013. Oh what happens it our tool does not neet
this requirement? Even more, how to proceed if we want to have an automatic way?

The answer is pretty simple, there is a strategy in EF we can use out-of-the-box [MigrateDatabaseToLatestVersion][4]

![_config.yml]({{ site.baseurl }}/images/posts/ionline/migrate-to-latest-azure.jpg)

On the first db read or write, EF will perform the migration task for us. It is useful also in case
the team follows continuous integration strategy, no manual step is needed.

## References
1. [Code First Migrations][2]
2. [Running & Scripting Migrations From Code][3]

[1]: http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
[2]: https://msdn.microsoft.com/en-us/data/jj591621.aspx
[3]: http://romiller.com/2012/02/09/running-scripting-migrations-from-code/
[4]: https://msdn.microsoft.com/en-us/library/hh829293(v=vs.113).aspx
