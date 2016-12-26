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


=> The need is to implement a subclass of SqlScriptExecutor, override method Log to handle outputs;

=> Then implement a custom of IUpgradeLog to save the result as expected;
If number of affected rows is important, it is required to override method Execute


# Second option: using Dapper !

With method QueryMultiple, all commands will be executed and outputs are kept in a GridReader object.
-> The need is to implement a behavior to serialize that object to expected format (CSV in this case)

![_config.yml](/images/posts/Dapper - QueryMultiple.png)

# And below is my Quick comparison

### DbUp
It is designed for upgrading databases, can track scripts have been run and skip running them on future upgrades

-> A custom table will be used in each database to store those scripts

-> Seems to be good in this case

### Dapper

Dapper is generally used for running scripts, not required much customization.

-> No behavior to keep track of executed scripts -> need to control that by ourself.


## References

1. [A question on SO - How to execute SQL files and handle results in .NET][1]
2. [DbUp on Github][2]
3. [Dapper Site][3]

[1]: http://stackoverflow.com/q/39221405/4903729
[2]: https://github.com/DbUp/DbUp
[3]: https://github.com/StackExchange/dapper-dot-net