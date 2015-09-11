---
layout: post
title: iOnline - application requirements
---

I have involved in building a web application that deployed on a could platform using different database system
from SQL to NoSQL database system. I will share here all steps we did to make it running.

The web application that manages messages written by any internet user. Below are the requirements

**Functional requirements**

- User can write a message by Markdown format, attach the message to a given category, seeing preview of the message.

- User can see number of messages by category

- User can list all messages in the system, or filter by a category. Extract from the message body a few words
to display on listing page.

- User can comment on any message with content from 

- User can view a message and all comments about it. The message should be displayed with Markdown format.

- User can search for messages by a keyword

- System must validate to ensure that message will not be posted by any robot

**Deployment requirements**

- Deploy the application on a cloud platform (Azure or any other ones like AppHarbor)

- System should be deployed easily on different database system including SQL Server, NoSQL (RavenDB/MongoDB).
Switch bitween different db systems easily.
