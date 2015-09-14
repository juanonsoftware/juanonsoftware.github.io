---
layout: post
title: Integrate Google reCaptcha into ASP.NET MVC form
tags: [aspnetmvc]
---


The purpose of this task is to avoid spam post on your site (actually on your forms).
We accept human post, not any robotic post ;)


## The library out there

Again, when having any task to do we should understand what it requires from both requirements view
and also technical view. Later, do some search to see if it has been done by any active guy in the globe,
even [we can implement it ourself][5] or [here][8].

With reCaptcha also, I found some useful libraries there [recaptcha-net][3], [reCAPTCH.MVC][4] and so forth.

## How to use RecaptchaNet

I choose this because it does not require any dependency (as version 1.3).

To have a detail guide from the creator, please go to there [documentation site][7]




## References
1. [What is reCAPTCHA][1]
2. [Google reCAPTCHA in ASP.NET MVC][2]

[1]: https://developers.google.com/recaptcha/
[2]: http://venkatbaggu.com/google-recaptcha-asp-net-mvc/
[3]: https://github.com/tanveery/recaptcha-net
[4]: https://www.nuget.org/packages/reCAPTCH.MVC/
[5]: http://stackoverflow.com/questions/4611122/how-to-implement-recaptcha-for-asp-net-mvc
[6]: http://recaptcha-net.mtd226.com/
[7]: http://recaptchamvc.apphb.com/
[8]: http://www.codeproject.com/Articles/874150/Google-reCAPTCHA-in-ASP-NET-MVC