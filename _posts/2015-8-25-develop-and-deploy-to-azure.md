---
layout: post
title: Azure - deployment and integration made easier
tags: [cloud]
---

Microsoft Azure is a cloud-platform which enables us to host not only web-application,
but also for long-running tasks such as data processing background task, periodic backup task...

If you are not having an account on Azure, try it today and I am sure that 
you will have very interesting experience.
[Go there](https://azure.microsoft.com/en-us/pricing/free-trial/) to register and get 200$ to buy
what service you want.

## Traditional hosting deployment

Many of us know and use traditional hosting service including shared-host, dedicated server...
but the deployment process is complex. Let's imaging this process

1. Develop application feature and doing tests to be sure it functions as expect
2. Build the application in release mode, create scripts that perform data upgrading
3. Use FTP to upload files (application packages) to the host
4. Execute data upgrade scripts
5. Verify all changes and enable them to customers

Those steps will take time (much or less depends on how complexity of the changes).
If we want have a release everyday it should be an amount of percentage compared to the whole cycle.
If we delay deployment, it may be risky also because the issue is not detect right when it appears.

## Azure deployment

Let's imaging we have below development process

1. Source code is managed by a cloud service (Github, BitBucket, VisualStudio Online...)
2. A commit will be done when a task is finished and tested on developer's local environment
3. The team is not hug, having 5 developers for example so they can make a dozen of commits each day.
4. We want to be sure that changes made from the whole team still work as expected. The easiest method
to ensure this is perform integration test as soon as possible.

With Azure (or other cloud services too) it handles this pretty well and simple.
First we create an application, then link it to a source code repository.
Whenever a new commit is raised, Azure will catch it and do the build, deploy new changes to the app
automatically. We focus on development process and make Azure handling the deployment.

## References
1. http://www.thoughtworks.com/continuous-delivery
2. http://martinfowler.com/articles/continuousIntegration.html
3. http://blog.teamtreehouse.com/use-continuous-integration-continuous-deployment
