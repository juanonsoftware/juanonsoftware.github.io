---
layout: post
title: Running tests on AppHarbor
tags: [studying, cloud]
---

I deploy a sample application called [SoftNews][1] on AppHarbor. It works pretty well without any purchase
as I still use free services (with small free RavenDB / Redis instances)

## Deployment when Tests green

We should only deploy "working code" to production. A lot of works relate to this step
but at leat we should do is ensuring all tests in the solution are passed.

I add unit test projects with some tests to see how it work.

If there is any test failing, the solution will be complied but not being deployed.

![_config.yml]({{ site.baseurl }}/images/posts/cloud/apphb-tests.png)

OK, now I need to fix failed test to make deployment work. And finally it works as expected,
all tests are passed and the last deployment is active (go prod)

![_config.yml]({{ site.baseurl }}/images/posts/cloud/apphb-tests-ok.png)



References

1. [AppHarbor KB][1]

[1]: http://softnews.apphb.com/
[2]: https://support.appharbor.com/kb/getting-started/running-unit-tests-after-build
