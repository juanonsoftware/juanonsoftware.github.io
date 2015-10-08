---
layout: post
title: WCF security - how to implement Authentication and Authorization
tags: [studying, wcf]
---

By default, WCF uses Windows to validate username / password. We can customize this process as well.
This post will describe how to to authenticate a client, then authorize its behavior.

## Use custom Username / Password authentication

First, we need to create a custom validator then plug it onto the WCF pipeline.

```
public class CustomUserNamePasswordValidator : UserNamePasswordValidator
{
	public override void Validate(string userName, string password)
	{
		// ... your logic
	}
}
```

Now we update the validator onto WCF behavior

![_config.yml]({{ site.baseurl }}/images/posts/wcf/behavior-with-custom-validator.png)

At client side, it must set username and password

![_config.yml]({{ site.baseurl }}/images/posts/wcf/set-username-password.png)

All the code is on [this github repository][1], you can get it to run locally.

*Note:* You must run the setup.bat file to configure a certification which will be used to encrypt the message.

## Authorization WCF methods

There are scenarios that we want to allow some specific clients to access a method, others are forbiden.
It's called WCF authorization. Suppose that you have a service up and running, below are steps needed to enable authorization.

1. Creating a custom principal

The purpose of this step is to control roles for any user.

```
public class CustomPrincipal : GenericPrincipal
{
	public CustomPrincipal(IIdentity identity)
		: base(identity, GetRoles(identity).ToArray())
	{
	}

	private static IEnumerable<string> GetRoles(IIdentity identity)
	{
		if (identity.Name == "hhoangvan")
		{
			return new List<string>()
			{
				"Administrators"
			};
		}

		return new List<string>()
		{
			"Users"
		};
	}
}
```

2. Creating an IAuthorizationPolicy implementation

This policy later will be set in configuration for service authorization.
A full example of a custom policy [is available on MSDN][5] but the simple one is available in the code.

Below is the configuration element for `serviceAuthorization`, two importans points are

- set principalPermissionMode="Custom"

- Add correct policyType

```
<behaviors>
  <serviceBehaviors>
	<behavior name="MessageServiceBehavior">
	  <serviceMetadata httpGetEnabled="true"/>
	  <serviceAuthorization principalPermissionMode="Custom">
		<authorizationPolicies>
		  <clear/>
		  <add policyType="ChattyServices.CustomAuthorizationPolicy, ChattyServices"/>
		</authorizationPolicies>
	  </serviceAuthorization>
```


References

- [Authorization in WCF][6]
- [How to: Use a Custom User Name and Password Validator][2]
- [WCF Service with custom username password authentication][3]
- [Message Security User Name][4]
- [Sample code][1]

[1]: https://github.com/juanonsoftware/70-487/tree/master/ChattyApp
[2]: https://msdn.microsoft.com/en-us/library/aa702565(v=vs.110).aspx
[3]: http://www.codeproject.com/Articles/96028/WCF-Service-with-custom-username-password-authenti
[4]: https://msdn.microsoft.com/en-us/library/ms752233(v=vs.110).aspx
[5]: https://msdn.microsoft.com/en-us/library/ms729794(v=vs.110).aspx
[6]: https://msdn.microsoft.com/en-us/library/ms733071(v=vs.110).aspx