---
layout: post
title: EF code first does not work after renaming a Migration
tags: [ef]
---

I've just solved a problem about creating database. The application was working well until now we delete the old database
to let Entity Framework creating a new one.

After more than one hour of debugging and looking around, the root cause seems comming from a simple action: rename the migration.

The rename could make it down on the list of migrations, but in reality it is still executed before other migrations.

That migration creates a stored procedure in bgp schema which has not been created yet. Below is the screenshot of error, 
but it is quite generic and pretty hard to identify exactly error could raise from.

**So... should create a new migration instead of renaming :)**

![_config.yml](/images/posts/ef/The-specified-schema-name-bgp-either-does-not-exist-or-you-do-not-have-permission-to-use-it.---Google-Chrome.jpg)
