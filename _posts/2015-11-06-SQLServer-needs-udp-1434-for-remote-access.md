---
layout: post
title: Configure SQL Server remote access on Windows Server 2K8
tags: [studying, cloud]
---

I got remote access issue of SQL Server 2K8 R2 on Windows Server 2K8 R2 which has firewall turned on. The fix took awhile
so this post will note all steps that have been performed.

## Server conditions

One server called `ServerA` have configured to be used as DB Server + Web Server. It has firewall turned on.
The application works as expected (it connects to db on the same machine)

Another server called `ServerB` has the same web app deployed, but it can't connect to db on the `ServerA`.

Some configurations have been performed such as [this post][1] still not working. Wooh, it seems strange,
because the port 1433 has been opened, TCP/IP has been enabled, the port has been verified in all IP addresses...

## What else inside can help?

Seems weired thing is hidden, I did more research and found a deeper post on SQL Protocols Blog,
the point it meantions I haven't known before.

The SQL Server uses another `UDP` port number 1434 before every connection to the server.

`Every time client makes a connection to SQL Server named instance, we will send a SSRP UDP packet to the server machine UDP port 1434.
We need this step to know configuration information of the SQL instance, e.g., protocols enabled, TCP port, pipe name etc.`

Finally, open UDP port 1434 help resolved the problem.


References

1. [How to enable remote connections in SQL Server 2008][1]

[1]: http://blogs.msdn.com/b/walzenbach/archive/2010/04/14/how-to-enable-remote-connections-in-sql-server-2008.aspx
[2]: http://blogs.msdn.com/b/sql_protocols/archive/2007/05/13/sql-network-interfaces-error-26-error-locating-server-instance-specified.aspx
