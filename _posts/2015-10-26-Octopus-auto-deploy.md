---
layout: post
title: Octopus - Automatic Deployment on Release Creation
tags: [studying]
---

It is possible to deploy new version of application to a specific environment on its new release creation.
This straightforward method makes testing / deployment easier as the environment usually is for dev / dev integration.

## Setup a Lifecycle

Instead of modifying the default lifecycle, it's better to create a custom one for your app(s).

In Lifecycle edition form, it allows to define Phases with choise `Deploy automatically...`.

![_config.yml]({{ site.baseurl }}/images/posts/octopus/phase-auto-deploy.png)

![_config.yml]({{ site.baseurl }}/images/posts/octopus/xbanking.png)

## Change Project's Lifecycle

Go to the Project / Process tab and check the current lifecycle applying to it

![_config.yml]({{ site.baseurl }}/images/posts/octopus/project-lifecycle.png)

Select the Lifecycle `XBanking` which has been setup at previous step. It's all we need to do on configuration steps.
When a new package is arriving, it will do different steps to:

- Create a new release

- Deploy the release to environments configured for automatically deploy.

Below command can be used to build and push the package to Octopus.

```
msbuild Demo.XBanking.sln 	/t:Build 
							/p:RunOctoPack=true
							/p:OctoPackPublishPackageToHttp=http://<your_server>:180/nuget/packages 
							/p:OctoPackPublishApiKey=API-XYZ
```

References

1. [Lifecycles][1]

[1]: http://docs.octopusdeploy.com/display/OD/Lifecycles
