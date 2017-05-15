---
layout: post
comments: true
title:  "Iterate Over Large Amount Of MongoDB Documents With RabbitMQ Work Queues"
date:   2017-05-14 03:24:36
categories: Development
---

So recently I need to iterate over and process a large amount of MongoDB documents, around 30,000 documents, each around 5~15MB in size, so it take quite a bit of time to process each individual document. 

And these documents need to be queried by different criteria, there are about 1000 individual queries with same query structure but different query value.

For the MVP I tried writing a simple Node.js script to loop over the documents asynchronously, fire up all queries and iterate on them all in one process(one event loop).

But I quickly realised it won't even work given unlimited time, as I counter lots of cursor not found issue, because MongoDB query cursor would time out, and because all queries(cursors) are created asynchronously and its execution order are not in my control, lots of queries won't get chance for running for a long period of time (longer than 10 minutes), and thus when finally given a chance, will raise an error and kill the process.

Given that using a cursor without timeout will be dangerous and a bad practice, and I want to speed up the process by using some parallel processing, I dicided to use a RabbitMQ work queue to solve the issue.

So I split the program into two parts, namely the producer part and consumer part; the producer part will query the MongoDB and get all document ids without processing them, then pipe the ids into the RabbitMQ worker queue. Then I launched 15 consumers with pm2 listening on the worker queue with simple round-robin dispatching to work these documents in parallel.

In this setup the producer did a short query and only retrieve all the ids without processing them, so it's fast; the consumers retrieve the document one by one via document id, thus it's also fast and impose a pretty low load on MongoDB. And the huge workload is roughly evenly distributed over the time and parallelised, so it's also good.

The setup could easily scale up by simply launching more consumers and could handle failure of running consumers by using message acknoledgement and thus re-delivery the un-acknoledged messages.
