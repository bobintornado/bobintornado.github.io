---
layout: post
comments: true
title:  "Implement A Url-shortener With Tracking And A Stat Endpoint"
date:   2017-07-15 03:24:36
categories: Development
---

Implement-A-Url-shortener-With-Tracking-And-A-Stat-Endpoint

Url shortener is a common web service, making long links short and sweet. There is quite A few service providers offer it, for examples Bitly, Google, and TinyURL.com.

In this post I'd like to show you how I implemented a basic url-shortening service and some of my thoughts along the way.

I start my implementation by googling how others have done it, and came across Jeff Atwood's post[URL Shortening: Hashes In Practice
](https://blog.codinghorror.com/url-shortening-hashes-in-practice/). And in that post he talks about his thoughts on the subject, how normal hashing couldn't work in this his guess, which is `...a simple iteration across every permutation of the available characters...`.  

It's a fun read and some great considerations, but the comments section taught me more. It gave me the idea that the key could be just a auto incremental number in a SQL table, and outputed in a string format. For example, id `37815042` could encoded as `2yFq2` in base62 format.

A table is added below to illustrate the idea 

| number  | value |
| ------------- | ------------- |
| 1  | 1  |
| 10  | a  |
| 100  | 1C  |
| 1000  | g8  |
| 10000  | 2Bi  |
| 100000  | q0U  |
| 1000000  | 4c92  |
| 10000000  | FXsk  |
| 100000000  | 6LAze  |


