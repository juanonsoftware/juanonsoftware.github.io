---
layout: post
title: How to implement pagination on ASP.NET MVC4
tags: [aspnetmvc]
---


Pagination !!! This task is very common as almost application requires pagination when listing data.
So how is a good direction to complete this task? I did it again today and below is how it has been completed.

## Any library out there?

Yes, there are many. I did not look around lot of them but doing search will give many results.
The two libraries I spent time to do some testing are [PagedList] [1] and [mvcpaging] [2].


## Implement pagination with PagedList

So I choosed PagedList for [ionline][3] project.

### Install nuget packages
Go to https://www.nuget.org/packages/PagedList.Mvc/ to install the package into your project.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/pagedlist-packages.jpg)

### Setup controller's Action

![_config.yml]({{ site.baseurl }}/images/posts/ionline/list-message-action.png)

The main part is bordered in green box, it does a test to check if the client supports ajax then only a partial view
will be returned or the whole view if not.

### The views

The list view simply renders the partial view on initial request.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/list-view.jpg)

The partial view contains an importal part is the *pager*, which will renders all pages with navigation links.
We define id of the _root_ element which holds content of the page.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/list-pager.jpg)

### Enable client Ajax

If the whole-page still being reloaded when you switch to other page, it means that the site is not configured
to enable unobtrusive JavaScript. A couple of actions required for this work.

- Set *UnobtrusiveJavaScriptEnabled* to true in web.config file
- Include the JQuery component [*jquery.unobtrusive-ajax*][4] to the project.

![_config.yml]({{ site.baseurl }}/images/posts/ionline/jquery-unobtrusive-ajax.jpg)

## References
1. [The PagedList repository] [1]
2. [MVC Paging Demo] [2]

[1]: https://github.com/TroyGoode/PagedList
[2]: http://mvcpaging.apphb.com/
[3]: http://ionline.azurewebsites.net
[4]: https://www.nuget.org/packages/Microsoft.jQuery.Unobtrusive.Ajax