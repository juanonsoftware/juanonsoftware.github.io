---
layout: post
title: EntityFramework - deploy custom stored procedures to Azure
tags: [ef, cloud]
---

Continuing on my [previous post regarding EF-Azure][1] for project [IOnline][2], I need to adapt
this requirement to: (1) Not using EF to count number of messages (total messages in the system and by categories)
and (2) Use [Dapper][3] for the data access layer as a replacement. So I will have to create some stored procedures
to make this works.

## 1. Create stored procedures and references to Dapper

Below is the SP needs to be created

![_config.yml]({{ site.baseurl }}/images/posts/ionline/count-messages.png)

The application will function after implemented a new service using Dapper.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/count-messages-with-dapper.png)

## 2. Enable migration

If the system runs on another database or Azure platform it doesn't work unless the script was deployed manually.
Again, it should be deployed automatically with continuous integration enabled. So a new migration is needed.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/count-messages-migration.jpg)

The above migration was empty when running `Add-Migration` command because no change has been made on the model.
To integrate the SP, new properties **UpScripts** and **DownScripts** are created which contains
sql statements to perform the migration.

And that's all from our side, the rest is EF it will perform `Update-Database` on the first read/write db operation.
We can take a coffee and see new affects after `push` those changes, Azure handles all other steps.

## References

1. [Code First Migrations and Stored Procedures][4]

[1]: {% post_url 2015-8-30-EntityFramework-code-first-automatic-migration-azure %}
[2]: https://github.com/juanonsoftware/ionline
[3]: https://github.com/StackExchange/dapper-dot-net
[4]: http://stackoverflow.com/a/16589152/744845
