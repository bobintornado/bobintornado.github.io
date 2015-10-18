---
layout: post
comments: true
title:  "A Dev's Story With Dropbox"
date:   2015-10-11 21:24:36
categories: Others
---

I really love Dropbox. 

Even it has caused one of the biggest and most mysterious bug for me for almost two weeks (the bug completely breaks my work flow as a developer).

I love Dropbox because it has significantly simplified my life by making files available everywhere. 

As a result, I never need to \"backup\" my files again, as they all live in the cloud; I configured my chrome downloading path to Dropbox, so I never need to \"transfer\" anything across devices anymore; I auto upload all my photos so that I could directly edit them on my Mac without \"copying\" them all over; I even put all my coding projects into Dropbox for my own convenience and passive backup.

Dropbox also hosts my 1Password key chain and a bunch of other stuffs. 

When I need to set up a new Mac, I basically download the Dropbox, and wait for it to finish syncing, then I am 95% done with my set up. It's literally this easy.

And it works. I mean it really \"**WORKS**\". It works so well that it has become part of my life, part of my assumptions regarding how things will work. I take \"Dropbox will work\" for granted.

What's more fascinating is that, Dropbox is such a product where the concept is really simple and easy to implement, but f**king hard to implement it well. And in my mind, Dropbox almost implements it perfectly.

As a developer myself, I couldn't admire anymore about how those engineers in Dropbox solve those hidden but hard questions in a really intuitive way. 

Secretly I want to work on those challenging problems myself.

However, the mysterious bug I mentioned at the beginning happened around two weeks ago, when I joined a new company. My new company, [Tinkerbox Studio](http://www.tinkerbox.com.sg/), is an awesome company, and this awesome company uses Dropbox, too! 

I happily get my Dropbox Business account and Dropbox creates a new folder for me. Dropbox also friendly tells me that now it manages two folders, one for myself, called `Dropbox (Personal)`, one  for work, called `Dropbox (Tinkerbox)`, and it also establishes a new symbol link from `Dropbox (Personal)` to the old `Dropbox`, so all my configurations should still work. 

Woo la! It's great! And indeed most of my configurations still work, such as my zsh aliases. But, the mysterious bug start popping up. 

My Webpack-dev-server refuses to detect changes, my Ember projects refuses to start, and my Rails Console refuses to fire up from time to time. 

You couldn't image what a terrible life I had when all these tools abandoned me. 

And I couldn't figure out why. I didn't figure this out for almost two weeks. 

And today, without the hope that it will work, I tried one more solution, which is to move my projects out of `Dropbox (Personal)`.

I moved my projects from `Dropbox (Personal)` folder to `Desktop` folder. 

And the miracle happened. Everything suddenly start working again. WTF.

So I back-trace the issue again, and read all errors messages I could find.

And I found out that it's due to the space between `Dropbox` and `(Personal)`. Yeah, it's the little space between two. 

I feel very happy that I solved the bug, and very frustrating because the root of all evils is the little space in my folder name, which also make a lot of senses, as it's really troublesome to work with space in folder name. 

I try using

`% rm -f Dropbox && mv "Dropbox (Personal)" Dropbox && ln -s Dropbox "Dropbox (Personal)"`

to cheat Dropbox, but partially failed. As it still works in synchronization, but the blue/green symbol on the right bottom corner of folders are all gone. :/ 

And I hate it. I feel insecure when I don't know when Dropbox has finished syncing. 

So I reverted back and decided to move all my projects from `Dropbox (Personal)` to `Desktop`. It's a painful decision as I really missed the passive backup benefit, but it seems it's the time to move on. 

On the bright side, I I will no longer see Dropbox eating more than 100% of my CPU whenever I am doing development anymore. (Cause thousands of files change every single minute when I am hacking stuff)

And I will miss the email from Dropbox saying \"You recently deleted a large number of files\", too. 

I really hope Dropbox is going to fix this (remove the space!) and also reduce CPU consumption, but I guess that will take some time.

So this is all my story with Dropbox as a dev, for now.