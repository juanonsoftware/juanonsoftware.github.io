---
layout: post
title: WCF Messaging Patterns
tags: [studying]
---

This post is about Messaging Patterns provided by WCF that I have learnt.
It includes one-way, request/reply, streaming, and duplex communication.

## OneWay pattern

It is about call and forget, we call a method on WCF service and the job is finished. We don't need to
care about the respond result or exception the might occurs.

To do that, we define a method as below, the `IsOneWay` property need to set and return type is `void`
```
[OperationContract(IsOneWay = true)]
void LogMessage(String messsage);
```

Actually, the server will send back a HttpStatusCode of 202 (Accepted) immediately before any processing begins.
But if you another OneWay method right after the first one, it will be blocked until the service finished processing the first call,
unless it was specifically call in async way.

## Streaming and duplex

WCF has two modes we can work with `Buffer` and `Streaming`

For large data, Streaming can benifit for performance and scalability.

Below are all TransferMode

----------------------------------------------------------------
|Buffered 			| The request and response messages are both buffered.|
----------------------------------------------------------------
|Streamed			|Both the request and response are streamed.|
----------------------------------------------------------------
|StreamedRequest	|The request message is streamed, and the response message is buffered.|
----------------------------------------------------------------
|StreamedResponse	|The request message is buffered, and the response message is streamed.|
----------------------------------------------------------------

And only below bindings support streamming out-of-box: 

- BasicHttpBinding
- NetTcpBinding
- NetNamedPipeBinding

## Request/Reply pattern

Any other WCF methods have this patterm by default.

## Duplex

This pattern supports the WCF service to send messages to client. The communication must occur within a context of session.
Also, it requires another service contract for CallbackContract.

References

- [Message Patterns in WCF Services][1]

[1]: https://msdn.microsoft.com/en-us/library/ff395349.aspx
[2]: http://www.dotnet-tricks.com/Tutorial/wcf/bWJI280913-Understanding-Message-Exchange-Patterns-(MEP)-in-WCF.html
[3]: http://www.c-sharpcorner.com/UploadFile/db2972/wcf-message-exchange-patterns-day-3/