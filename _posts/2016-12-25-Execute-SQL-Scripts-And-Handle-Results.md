---
layout: post
title: How to execute one or many SQL files and handle all outputs in .NET
tags: [studying]
---

Have considered about this requirement for a while and after some studies I found few options. Here are some:

# First option: with DbUp !

It is very nice for this, it can read sql scripts from various sources (file system, embeded in assembly...), then execute each script
in the list (parse each script into a list of commands before executing), then it can log the script's outputs.

But looked into SqlScriptExecutor, it also writes other infos such as script name, error exception into the log source beside script's results.

![_config.yml](/images/posts/DbUp - SqlScriptExecutor.png)


=> The need is to implement a subclass of [SqlScriptExecutor][4], override method Log to handle the outputs;

=> Then implement a custom of IUpgradeLog to save the result as expected;
If number of affected rows is important, it is required to override method Execute


# Second option: using Dapper !

With method QueryMultiple, all commands will be executed and outputs are kept in a GridReader object.
-> The need is to implement a behavior to serialize that object to expected format (CSV in this case)

![_config.yml](/images/posts/Dapper - QueryMultiple.png)

# 3rd option: via [sqlcmd.exe][5] utility

This is nice as the input and output can be customize by many parameters.

But seems difficult to manage transaction (per script, per all scripts); not able to run on other DBMS such as MySQL.

# And below is my Quick comparison

### 1. DbUp
It is designed for upgrading databases, can track scripts have been run and skip running them on future upgrades

-> A custom table will be used in each database to store those scripts

-> Seems good in this case

### 2. Dapper

Dapper is generally used for running scripts, not required much customization.

-> No behavior to keep track of executed scripts -> need to control that by ourself.


## References

1. [A question on SO - How to execute SQL files and handle results in .NET][1]
2. [DbUp on Github][2]
3. [Dapper Site][3]

[1]: http://stackoverflow.com/q/39221405/4903729
[2]: https://github.com/DbUp/DbUp
[3]: https://github.com/StackExchange/dapper-dot-net
[4]: https://github.com/DbUp/DbUp/blob/master/src/DbUp/Support/SqlServer/SqlScriptExecutor.cs
[5]: https://msdn.microsoft.com/en-us/library/ms180944.aspx