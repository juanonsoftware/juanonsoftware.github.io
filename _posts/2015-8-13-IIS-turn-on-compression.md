---
layout: post
title: Enable compression for website
tags: [aspnetmvc]
---

We can enable content compression by code or by features provided by IIS. Also it can be enabled on both.

## Doing compression at IIS level

I think this is a more convenient way to handle this, because no coding requires then no potential bugs.
Also it can be changed easily by changing some parameters without any rebuild.

<system.webServer>
    <urlCompression doStaticCompression="true" doDynamicCompression="true" />
</system.webServer>

## Doing compression at Application level

This attribute is taken from SO site. It is similar to the one on [this page][3]

public class CompressAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {

        var encodingsAccepted = filterContext.HttpContext.Request.Headers["Accept-Encoding"];
        if (string.IsNullOrEmpty(encodingsAccepted)) return;

        encodingsAccepted = encodingsAccepted.ToLowerInvariant();
        var response = filterContext.HttpContext.Response;

        if (encodingsAccepted.Contains("gzip"))
        {
            response.AppendHeader("Content-encoding", "gzip");
            response.Filter = new GZipStream(response.Filter, CompressionMode.Compress);
        }
		else if (encodingsAccepted.Contains("deflate"))
        {
            response.AppendHeader("Content-encoding", "deflate");
            response.Filter = new DeflateStream(response.Filter, CompressionMode.Compress);
        }
    }
}

## References
1. [How do I enable gzip compression when using MVC3 on IIS7][1]
2. [URL Compression][2]
3. [GZip/Deflate Compression in ASP.NET MVC][3]

[1]: http://stackoverflow.com/questions/6992524/how-do-i-enable-gzip-compression-when-using-mvc3-on-iis7
[2]: http://www.iis.net/configreference/system.webserver/urlcompression
[3]: http://weblog.west-wind.com/posts/2012/Apr/28/GZipDeflate-Compression-in-ASPNET-MVC
