---
layout: post
title: Empty folder issues on AppHarbor
tags: [studying, cloud]
---

I got an issue when deploying an application to AppHarbor cloud Platform regarding empty folder (!).
It is a serious one because the application crash after deployment even with a experimental project.

There is one exception has been captured in the log

![_config.yml]({{ site.baseurl }}/images/posts/cloud/apphb-exception-empty-folder.png)

The folder was being used by `ScriptBundle`, it exists locally but has not been created by deployment tool.

So to be sure for it and similar ones, I created a `Readme.txt` file for each and the issue is fixed then.

![_config.yml]({{ site.baseurl }}/images/posts/cloud/readme-files.png)