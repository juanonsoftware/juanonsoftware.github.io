---
layout: post
title: Start using Octopus to deploy apps
tags: [studying]
---

## Setup Octopus

[Octopus requirements][1] are important, it requires Windows Server 2K8+. It shouldn't be installed
on any Windows terminal OS (eg Windows 7) because after that it doesn't work correctly.

I had some errors with Octopus on Windows 7, eg

```
Push-Location : Cannot find drive. A drive with the name 'IIS' does not exist.
17:12:43Error
At D:\Octopus\Applications\Test - Local\Demo.XBanking\1.1.0.0_1\Octopus.Feature
17:12:43Error
s.IISWebSite_BeforePostDeploy.ps1:214 char:6
```

The same thing with Octopus Tentacle (check it requirements [there][2]), it can be installed correctly on Windows 7 but doesn't work correctly
when I connect from Windows Server 2K8 (Octopus Server).

## Using Octopus

1. Creating Environments

Just put some information into the form the create an Environment.

![_config.yml]({{ site.baseurl }}/images/posts/octopus/environment.png)

More than one environment can be created depending on requirements of the applications.

2. Creating Projects

Just put some information into the form the create a Project.

![_config.yml]({{ site.baseurl }}/images/posts/octopus/project.png)

3. Define process of deployment for your project

Process of deployment can be devided into multiple steps. Each project may require different steps than others.

Below are example steps of a XBanking web applicaiton:

- (1) Sends an email to a list of people
- (2) Deploys the application from a package to web server(s)
- (3) Sends a failure email if the deployment was failed
- (4) Sends a success email if the deployment was success

![_config.yml]({{ site.baseurl }}/images/posts/octopus/project.png)

4. Configure SMTP

Because the deployment process contains some mailing steps, so it requires to provide SMTP information.
It will ask you to do some testing to besure that the setting is good.

![_config.yml]({{ site.baseurl }}/images/posts/octopus/smtp.png)

5. Build package for your application

There are a [number of options][5] to create NuGet package of your application. In this document,
I use [OctoPack][6] way. That requires to install OctoPack package into the running project
(Web Application, Console / Windows Application...)

After that we can use MSBuild command to

- Build the solution (line 1)

- Generate a package (line 2)

- Publish the package to Octopus Built-in NuGet server (line 3 + 4)

```
msbuild Demo.XBanking.sln 	/t:Build 
							/p:RunOctoPack=true
							/p:OctoPackPublishPackageToHttp=http://<your_server>:180/nuget/packages 
							/p:OctoPackPublishApiKey=API-XYZ
```

After above step, the should be displayed in Octopus feed

![_config.yml]({{ site.baseurl }}/images/posts/octopus/library.png)

6. Creating Releases and make deployment

Now it's ready to create project release(s) and deploy to target environment(s).
Octopus should send emails regarding each deployment status.

![_config.yml]({{ site.baseurl }}/images/posts/octopus/email-status.png)

References

1. [Octopus Key Concepts][7]
2. 


[1]: http://docs.octopusdeploy.com/display/OD/Installing+Octopus
[2]: http://docs.octopusdeploy.com/display/OD/Installing+Tentacles
[3]: http://ianpaullin.com/2014/06/03/octopus-deployment-basics/
[4]: http://docs.octopusdeploy.com/display/OD/Deploying+packages
[5]: http://docs.octopusdeploy.com/display/OD/Packaging+applications
[6]: http://docs.octopusdeploy.com/display/OD/Using+OctoPack
[7]: http://docs.octopusdeploy.com/display/OD/Key+Concepts