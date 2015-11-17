---
layout: post
title: Sign-in via Twitter account - common issues
tags: [studying]
---

Today I integrate my application with Twitter and user now can sign-in by their Twitter account.
It's not so hard with support of the libraries `Microsoft.Owin.Security.Twitter` or `Owin.Security.Providers`.
Beside, there are some **issues** soonly happens.

##1. Needed a full-qualified domain

When creating an `application` in [Twitter Apps][1], we must provide a FQDN such as webapioauth.apphb.com
because it does not support localhost. OK so we can do that by modify the `host` file in our dev environment.

##2. A callback URL

The field **Callback URL** is not required when creating a Twitter App, but withit enter a valid value, an error appears during authenticating.

![_config.yml](//i.stack.imgur.com/0WUWY.png)

##3. Certificate issue

One other issue is about remote certificate exception. But there were some guys who facing with it and ca help us.
Taking values from [this answer][2] will help to fix the problem.

OK, now you finish also. Our users can authenticate by their Twitter accounts.


[1]: https://apps.twitter.com/
[2]: http://stackoverflow.com/a/32730571/744845