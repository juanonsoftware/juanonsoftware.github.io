---
layout: post
title: Asynchronous programming with example
tags: [studying]
---

Asynchronous programming will help us improve performance and responsiveness of the app.
The new architecture containing `async` and `await` keywords does almost difficult job and let developers
write code similarly to synchronous code.

# First example with a Console Application

```
class Program
{
	static void Main(string[] args)
	{
		ProcessRecords();
		Console.WriteLine("Wait for the result...");
		Console.ReadLine();
	}

	static async void ProcessDbAsync()
	{
		var repository = new Repository();
		var records = await repository.CountRecordsAsync();
		Console.WriteLine("Records: " + records);
	}
}
```

If removing all async and await keywords, we expect a result as below:

```
Records: 10
Wait for the result...
```

With async / await, we have this result after some seconds of wating for ProcessDbAsync to be finished

![_config.yml]({{ site.baseurl }}/images/posts/async/console-app-async.png)

The `CountRecords` is a long running task and it may take times to finish. `ProcessDbAsync` is an async method,
it immediately returns control to the `Main`. That's reason we see the like `Wait for the result...`.


Inside method `ProcessDbAsync()`, when it reaches `await` keyword, it waits for `CountRecordsAsync` to finished.
When it finished, result appears on console.


References

- [Asynchronous Programming with Async and Await][1]

[1]: https://msdn.microsoft.com/en-us/library/vstudio/hh191443(v=vs.110).aspx