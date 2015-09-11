---
layout: post
title: Applying DI / IoC into ASP.NET MVC project using Autofac
---

My website is up and running well even it has not been implemented any dependency injection container.
The result as this moment is that if a service has a dependency, it depends on the dependency implementation, they are coupling.
A better design is that the service should depend on dependency's abstraction. So I will apply Dependency Injection and bring a DI container into it.

There are many good DI containers ([Performance comparison][3]) and I will choose [Autofac][2] for this project.

## A repository's constructor

![_config.yml]({{ site.baseurl }}/images/posts/di/service-before-and-after.png)

The left side is the code before applying DI

- Service depends on an implementation `AppDbContext`

- Service manages instance lifetime of `AppDbContext`

Code on the right side has been refactored

- Service depends on `IDbContext`, an abstraction of `AppDbContext`

- Service focus on its logic, not needed to manage instances of all dependencies

Similarly, we have the same thing on controllers and other services

![_config.yml]({{ site.baseurl }}/images/posts/di/controller-before-after.png)

## Working with Autofac

To start with Autofac, there are some useful articles

**1. Install Autofac ASP.NET MVC4 Integration** (project is based on ASP.NET MVC4) - [nuget package][4]

**2. Create an Autofac Module**
A Module allows us to register services and how it will be instanciated. This is an example

![_config.yml]({{ site.baseurl }}/images/posts/di/ef-module.png)

**3. Make Autofac working**

For detail, a good document is available [there][5]

Finally, this method will integrate Autofac into ASP.NET engine. The depedencies will be injected
into controllers, services, repositories following what has been registered on Autofac's Modules

![_config.yml]({{ site.baseurl }}/images/posts/di/apply-settings.png)


[1]: http://www.codeproject.com/Articles/808894/IoC-in-ASP-NET-MVC-using-Autofac
[2]: http://autofac.org/
[3]: http://www.palmmedia.de/blog/2011/8/30/ioc-container-benchmark-performance-comparison
[4]: https://www.nuget.org/packages/Autofac.Mvc4/
[5]: http://autofac.readthedocs.org/en/latest/integration/mvc.html#quick-start
[6]: https://github.com/juanonsoftware/ionline/commit/286d82bd37746d117443548edb1fb02558699646