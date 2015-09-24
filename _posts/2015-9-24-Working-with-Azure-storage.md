---
layout: post
title: Getting started with Azure Storage
tags: [studying, cloud]
---

In this post I will share my little POC on Azure Cloud Storage including Blob / Queue.
There are other ones like DocumentDB / Azure Tables / SQL Database, I had worked with but another post will cover these parts later.
All code samples can be found in [this repository on github][1]

##The requirements

So we will cover below points about Azure Storage

**Blob**

- Upload a file to server, store the file in Azure Blob

- Create Blob Containers

- List all Containers

- List all files (blobs) by a Container

**Queue**

- Create new queue

- Add new messages to the queue

- Process all messages in the queue (peek the message, process it and stored into an Append Blob, delete message)

##Set up a project

You can create a blank MVC project then install this package WindowsAzure.Storage (current version is 5.0.2).

Go to Azure Portal to get `Account Name`, `Account Key` to build connection string. This is an example

![_config.yml]({{ site.baseurl }}/images/posts/azure/azure-storage-account.png)

##Creating client

To work with any Azure Storage service, below steps are required

- Parse connection string to `CloudStorageAccount`

- Create a client, for Azure Blob it is `CloudBlobClient`, and `CloudQueueClient` is for Azure Queue...

##Work with Blob

Below is Blob service concepts

![_config.yml](https://acomdpsstorage.blob.core.windows.net/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/20150902063132/includes/storage-blob-concepts-include/blob1.jpg)

To store any files to Blob, we must manage to create containers then blobs will be added in each.

- Block blob has 200GB limit.
- Append blob is similar to `Block blobs` and is optimized for append operations.
- Page blobs ha 1TB limit, is more efficient for frequent read/write operations.

For details, [this link][6] has described them. Below are some scenarios on Blobs

*List all containers*

            var containers = _blobClient.ListContainers(detailsIncluded: ContainerListingDetails.Metadata);
            var containersDict = containers.ToDictionary(x => x.Uri, x => x.Name);

			
*Get all blobs in a container*

            var container = _blobClient.GetContainerReference(uri);
            var blobs = container.ListBlobs(useFlatBlobListing: true);

Result of the ListBlobs method are a collection of `IListBlobItem` but it will actually be one of these concrete types
`BlockBlob`, `AppendBlob`, `PageBlob`.

*Upload data to blob*

This code does upload a file stream from `HttpPostedFileBase` to a blob.

                var container = _blobClient.GetContainerReference(DateTime.Today.ToString("yyyy-MM"));
                var blob = container.GetBlockBlobReference(string.Format("{0}_{1}", DateTime.Now.ToFileTime(), fileName));
                blob.UploadFromStream(file.InputStream);


##Work with Queue

For detail, you can refer to [this document][2] with deep information.

*Add message to the Queue*

- We must ensure the queue exists before working with, below is an example

            var cloudQueue = _queueClient.GetQueueReference(DateTime.Now.ToString("yyyy-MM"));
            cloudQueue.CreateIfNotExists();

- Then we can freely add any message into it

            var newMessage = new Message()
            {
                CreatedAt = DateTime.Now,
                Content = "A sample message"
            };

            cloudQueue.AddMessage(new CloudQueueMessage(newMessage.Serialize()));

*newMessage.Serialize()* is an extension method provided by `Rabbit.SerializationMaster` framework.
You can use strategies included in that package, or you can install an concrete implementation such as `Rabbit.SerializationMaster.ServiceStack`.
They're available on nuget.org.

*Process messages in the queue*

To adapt the requirements, we will need to:

1. Get the next message in the queue
2. If there is any, process the message. Otherwise, ends the loop.
3. Add the result by appending to another Append Blob
4. Delete message from the queue
5. Continue step 1

Below is completed code for all above steps

            // Get the queue on Azure
            var cloudQueue = _queueClient.GetQueueReference(name);

            // Prepare an Append Blob
            var container = _blobClient.GetContainerReference(DateTime.Now.ToString("yyyy-MM"));
            container.CreateIfNotExists();

            var blob = container.GetAppendBlobReference("Messages");
            if (!blob.Exists())
            {
                blob.CreateOrReplace();
            }

            while (true)
            {
                // Peek a message
                var queueMessage = cloudQueue.PeekMessage();
                if (queueMessage == null)
                {
                    break;
                }

                // Process the message
                var message = queueMessage.AsString.Deserialize<Message>();
                message.ProcessedAt = DateTime.Now;

                // Append to a storage
                blob.AppendText(message.Serialize());

                // Delete message from the queue
                queueMessage = cloudQueue.GetMessage();
                cloudQueue.DeleteMessage(queueMessage);
            }

			
References
- [How to use Queue storage from .NET][2]

[1]: https://github.com/juanonsoftware/azure-storage-demo
[2]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-queues/
[3]: https://msdn.microsoft.com/en-us/library/azure/mt427365.aspx
[4]: http://social.technet.microsoft.com/wiki/contents/articles/1674.data-storage-offerings-on-the-azure-platform.aspx
[5]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/
[6]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/#blob-service-concepts
