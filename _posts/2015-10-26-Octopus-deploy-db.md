---
layout: post
title: Octopus - Deploy DB automatically with EF migration
tags: [studying]
---

This post will explain how to automatically deploy database for an application in Octopus with EntityFramework.

## Build Data project

The data project contains all entities and a data context which inherits from DbContext and EF migrations.
`In real world application, it shouldn't be like this. All the entities should be placed in a different assembly.
DbContext and Migrations should be in another assembly.`

Suppose that the project has EF installed

### Configuration entity

```
public class Configuration
{
	public Configuration()
	{
		Id = Guid.NewGuid();
	}

	public Guid Id { get; private set; }

	public string Version { get; set; }

	public string ReleaseNotes { get; set; }

	public DateTime SetupDate { get; set; }
}
```

### XBankingDbContext context
```
public class XBankingDbContext : DbContext
{
	public XBankingDbContext()
		: base("name=XBankingDatabase")
	{
	}

	public IDbSet<Configuration> Configurations { get; set; }
}
```

### Enable migration

Use this command `Enable-Migrations` to enable Migrations in PM console.

Use this command `Add-Migration` to add the first migration.

Override the method Seed of `Migrations/Configuration` class as below, which will add 

```
protected override void Seed(XBankingDbContext context)
{
	var version = typeof(Configuration).Assembly.GetName().Version.ToString();

	if (!context.Configurations.Any(c => c.Version == version))
	{
		context.Configurations.Add(new Data.Configuration()
		{
			SetupDate = DateTime.Today,
			Version = version
		});
	}

	context.SaveChanges();
}
```

## Packaging and push to Octopus

Use OctoPack to pack `Data package`, then use below command to publish the package to Octopus.
Also add `migrate.exe` from EF package as a link into the project.

```
msbuild Demo.XBanking.sln 	/t:Build 
							/p:Configuration=Release
							/p:RunOctoPack=true
							/p:OctoPackPublishPackageToHttp=http://<your_server>:180/nuget/packages 
							/p:OctoPackPublishApiKey=API-XYZ
```

## Config deployment step in Octopus

Create a `Deploy a NuGet package` step, choose a good package (suppose it is Demo.XBanking.Data), then
enable `Custom deployment scripts` feature for the step.

To make `migrate.exe` work, pust some PowerShell commands into `Post-deployment script` as below

```
Write-Host "Connection String: <"$ConnectionString">"

$fullPath = (Join-Path $OctopusOriginalPackageDirectoryPath "migrate.exe")
Write-Host "Migrate Path:" $fullPath

Write-Host "Working Dir: "$(get-location)

# Run the migration utility
& "$fullPath" Demo.XBanking.Data.dll /startUpConfigurationFile=Demo.XBanking.Data.dll.config /connectionString=$ConnectionString /verbose | Write-Host
```

There are some variables should be configured in Octopus, (check [references][1] for more information)

- $ConnectionString : connection to the database. If not specifying, the tool get information from .config file.

Here is an result screenshot

![_config.yml]({{ site.baseurl }}/images/posts/octopus/deploydb.png)


References

1. [Migrate.exe references][1]

[1]: https://msdn.microsoft.com/en-us/data/jj618307.aspx
