---
layout: post
comments: true
title:  "Implement A Url-shortener With Tracking And A Stat Endpoint"
date:   2018-07-15 03:24:36
categories: Development
---

Url shortener is a typical web service, making long links short and sweet. There are quite a few service providers for it, e.g. Bitly, Google, and TinyURL.com.

In this post, I'd like to show you how I implemented a basic URL-shortening service and some of my thoughts along the way.

I start my implementation by googling how others have done it and came across Jeff Atwood's post[URL Shortening: Hashes In Practice
](https://blog.codinghorror.com/url-shortening-hashes-in-practice/). And in that post, he talked about his thoughts on the subject matter, such as how normal hashing couldn't work and his guess, which is `My guess is the aggressive URL shortening services are doing a simple iteration across every permutation of the available characters they have as the URLs come in.`.  

It's a fun read and some great considerations, but the comments section taught me more. It gave me the idea that the key could be just an auto incremental number in a SQL table and outputted in a string format. For example, id `37815042` could be encoded as `2yFq2` in base62(0-9, a-z, A-Z) format.

A table is added below to illustrate the idea. 

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


So for every single new URL, the database will save the value and keep it unique, assign a simple incremental id to the URL, and lastly return the base62 result as the *tiny* URL.

A sample implementation in Rails 5 could be found at [here](https://github.com/bobintornado/url-shortener).
