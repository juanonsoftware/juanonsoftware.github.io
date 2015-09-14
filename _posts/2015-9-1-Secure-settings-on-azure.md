---
layout: post
title: Secure configuration values on Azure
tags: [cloud]
---

If an application will be deployed on Azure we can secure sensitive application settings
(include AppSettings and ConnectionStrings) easily with just a little work for doing configuration and
nothing is required to change on code.

## 1. Settings on Web.config

In the sample application, there are two keys with local value on Web.config file. After deployed to Azure
it views as below

![_config.yml]({{ site.baseurl }}/images/posts/azure/value-from-web.config.png)

## 2. Create settings on Azure portal

On Azure, environment settings will override application local values if there is the same thing set on Azure portal.
Let's do a simple work to verify this behavior ;)

![_config.yml]({{ site.baseurl }}/images/posts/azure/azure-settings.png)

An app setting key and a connection string have been set, we can see them loaded on application as below:

![_config.yml]({{ site.baseurl }}/images/posts/azure/display-value-from-azure-env.png)


## 3. Conclusion

*Do not put production settings (often contains sensitive information) on Web.config, it has to be configured on Azure portal*

## References

1. [Windows Azure Web Sites: How Application Strings and Connection Strings Work][1]

[1]: http://azure.microsoft.com/en-us/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/