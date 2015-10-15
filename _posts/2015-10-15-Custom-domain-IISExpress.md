---
layout: post
title: Use custom domain with IISExpress
tags: [studying, aspnetmvc]
---

The testing purpose of [webapioauth.apphb.com][1] requires to use a FQDN rather than using localhost as usual.
So it requires to do some modification on IISExpress settings. Below steps are easily to make work.

1. Besure that we use IISExpress in Visual Studio

2. Change Project Url to the one we expect

![_config.yml]({{ site.baseurl }}/images/posts/vs/project-url.png)

3. Modify the side definition in `%USERPROFILE%\My Documents\IISExpress\config\applicationhost.config` as below,
two important attributes should be updated

physicalPath = D:\Projects\webapi-oauth-intergration\WebApiExternalAuth

bindingInformation="*:40678:development.com"

```
<site name="WebApiExternalAuthDev" id="999122">
	<application path="/" applicationPool="Clr4IntegratedAppPool">
		<virtualDirectory path="/" physicalPath="D:\Projects\webapi-oauth-intergration\WebApiExternalAuth" />
	</application>
	<bindings>
		<binding protocol="http" bindingInformation="*:40678:development.com" />
	</bindings>
</site>
```

4. Modify the host file, add a new line to maps your machine with development.com

`127.0.0.1 development.com`

[1]: http://webapioauth.apphb.com/
[2]: http://stackoverflow.com/a/5186680/744845