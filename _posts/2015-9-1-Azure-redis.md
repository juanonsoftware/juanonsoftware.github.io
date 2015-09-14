---
layout: post
title: Use Redis on Azure for caching
tags: [cloud, nosql]
---

[Redis][4] is a suppoer fast NoSQL data store, it can be used to cache our very high loaded data.
This article will show in detail how to setup and configure Redis Cache on Azure and its usage.

## 1. Configure and setup Redis Cache on Azure

After having an account on Azure, we can add a cloud service (for details, please go through [this instruction][1]).
Finally, choose a package as below:

![_config.yml]({{ site.baseurl }}/images/posts/azure/create-redis-service.png)

By default, it requires connection via SSL channel. To adjust these settings including port numbers, SSL or non-SSL,
take a look on below screen:

![_config.yml]({{ site.baseurl }}/images/posts/azure/redis-change-access.png)

When this step is done, it's ready to be used, so move to the second step.

## 2. Use Redis Cache on Azure

Let's take a look on below screen, few steps are done in this order
1. Setup a connection options
2. Do connect to the endpoint
3. Get from cache by a key
4. If the key is not exists, try to set it to a value

![_config.yml]({{ site.baseurl }}/images/posts/azure/mvc-redis.png)

**When having an error like this, please check the port number again if it is blocked on your network**

![_config.yml]({{ site.baseurl }}/images/posts/azure/redis-connection-port-not-allowed.png)

## References

1. [How to Use Azure Redis Cache][1]
2. [Windows Azure Web Sites: How Application Strings and Connection Strings Work][2]
3. [Config Redis Connection][5]
4. [Caching data in Azure Redis Cache][6]

[1]: https://azure.microsoft.com/en-us/documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/
[2]: http://azure.microsoft.com/en-us/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/
[3]: http://stackoverflow.com/questions/23773279/connecting-to-azure-redis-cache
[4]: http://redis.io/
[5]: http://stackoverflow.com/questions/23773279/connecting-to-azure-redis-cache
[6]: https://msdn.microsoft.com/en-us/library/azure/dn690521.aspx