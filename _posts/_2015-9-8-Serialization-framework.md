---
layout: post
title: A serialization framework
---



## Available ways

There are some ways already to make it working from [Two Methods][1] or [HTTPS only on Windows Azure Web Sites][2]

![_config.yml]({{ site.baseurl }}/images/posts/config-https-only.png)

but it doesn't fit to my case : development environment shouldn't be effected, it changes only the production.
Then the new attribute was born ;)

## RequireHttpsInProductionAttribute

So it adapts for these requirements
- Locally should not be effected by default, otherwise it needs to be set.
- When having some test environments (based on hostname) it should exclude this check.

Then it will be registered as usual
```
filters.Add(new RequireHttpsInProductionAttribute());
```

Below is main part of the attribute, and [full version here][3]

![_config.yml]({{ site.baseurl }}/images/posts/require-https-production-attribute.png)



[1]: http://blog.smarx.com/posts/redirecting-to-https-in-windows-azure-two-methods
[2]: http://blogs.msdn.com/b/benjaminperkins/archive/2014/01/07/https-only-on-windows-azure-web-sites.aspx
[3]: https://github.com/netvietdev/RabbitFoundation/blob/master/Rabbit.Web.Mvc/Filters/RequireHttpsInProductionAttribute.cs