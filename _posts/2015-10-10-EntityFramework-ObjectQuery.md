---
layout: post
title: EntityFramework - advanced query
tags: [studying, ef]
---

EntityFramework contains ObjectQuery type which supports to perform advanced and dynamic query for entity with SQL-like syntax.

## Create ObjectQuery instance

`ObjectQuery` can be instantiated through `DbContext` such as

```
var queryString = "SELECT VALUE p FROM Projects as p WHERE p.Id > @projectId";
var query = ((IObjectContextAdapter)context).ObjectContext.CreateQuery<Project>(queryString, new ObjectParameter("projectId", 0));
```

Now we can apply filters (Top, Skip...) on the query such as

**Get two fields only**

```
var records = objectQuery.Select("it.Id, it.Name");
```

**Applying pagination**

The Skip accepts first parameters to order result befor filtering

```
var records = objectQuery.Select("it.Id, it.Name").Skip("it.Name", "2").Top("2");
```

By this way EF generates SQL command and executes via `sp_executesql`

```
exec sp_executesql N'SELECT 
[Extent1].[Id] AS [Id], 
[Extent1].[Name] AS [Name], 
[Extent1].[Employee_Id] AS [Employee_Id]
FROM [dbo].[Projects] AS [Extent1]
WHERE [Extent1].[Id] > @projectId',N'@projectId int',@projectId=0
go
```

## LINQ to Entities

We can also use LINQ syntax to perform filter

```
var results =
	context.Projects.Where(p => p.Id > 0)
		.Select(x => new { x.Id, x.Name })
		.OrderByDescending(x => x.Name)
		.Skip(2)
		.Take(2)
		.ToList();
```

This way it does the same work but it's not dynamic. The fields we must be specified in code at compilation time.

EF generates SQL command and performs against db system

```
SELECT TOP (2) 
[Filter1].[Id] AS [Id], 
[Filter1].[Name] AS [Name]
FROM ( SELECT [Extent1].[Id] AS [Id], [Extent1].[Name] AS [Name], row_number() OVER (ORDER BY [Extent1].[Name] DESC) AS [row_number]
	FROM [dbo].[Projects] AS [Extent1]
	WHERE [Extent1].[Id] > 0
)  AS [Filter1]
WHERE [Filter1].[row_number] > 2
ORDER BY [Filter1].[Name] DESC
go
```

References

- [Object Queries][1]
- [Query Builder Methods][2]
- [ObjectQuery<T> Class][3]

[1]: https://msdn.microsoft.com/en-us/library/bb896241(v=vs.100).aspx
[2]: https://msdn.microsoft.com/en-us/library/bb896238(v=vs.100).aspx
[3]: https://msdn.microsoft.com/en-us/library/bb345303.aspx