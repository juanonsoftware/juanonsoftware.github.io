---
layout: post
title: EntityFramework - Mapping entities better
tags: [ef]
---

Basis of mapping in Entity Framework, how to map a POCO to table and customize mapping using Fluent API.


##1. Primary key
By default, it will take property named `ID` or `Id` or `<ClassName>Id` such as `UserId`, `PostId`.
([Using the DbContext API link][4])

To specify it manually, this is a syntax
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);

##2. Configuring on a Property

```
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
modelBuilder.Entity<Department>().Property(t => t.Name).HasColumnName("DepartmentName");
```

##3. Configuring Properties on a Complex Type

**Specifying That a Class Is a Complex Type**
```
modelBuilder.ComplexType<Details>();
```

```
modelBuilder.ComplexType<Details>() 
    .Property(t => t.Location) 
    .HasMaxLength(20);

modelBuilder.Entity<OnsiteCourse>() 
    .Property(t => t.Details.Location) 
    .HasMaxLength(20);
```

##4. Inheritance

*Mapping the Table-Per-Type (TPT)*

```
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```

*Mapping the Table-Per-Concrete Class (TPC*

```
modelBuilder.Entity<OnsiteCourse>().Map(m => 
{ 
    m.MapInheritedProperties(); 
    m.ToTable("OnsiteCourse"); 
}); 
 
modelBuilder.Entity<OnlineCourse>().Map(m => 
{ 
    m.MapInheritedProperties(); 
    m.ToTable("OnlineCourse"); 
});
```


## References
1. [Configuring/Mapping Properties and Types with the Fluent API][1]
2. [Entity Framework Code First][2]
3. [Managing DbContext the right way with Entity Framework 6: an in-depth guide][3]

[1]: https://msdn.microsoft.com/en-us/data/jj591617.aspx
[2]: http://www.codeproject.com/Tips/661053/Entity-Framework-Code-First-Map
[3]: http://mehdi.me/ambient-dbcontext-in-ef6/
[4]: https://msdn.microsoft.com/en-us/data/gg192989.aspx