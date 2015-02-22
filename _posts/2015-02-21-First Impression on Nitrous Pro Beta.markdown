---
layout: post
comments: true
title:  "First Impression on Nitrous Pro Beta"
date:   2015-02-21 21:47:36
categories: Development
---

So a few days ago I got this "Nitrous Pro Private Beta" invitation email from the Nitrous team with a $50 credit. Since I am using Nitrous recently on a few small projects, and I'm actually a paid user, so I decide to go ahead to give a try. Not mentioning they now give you **root access**!

You could also go to their new website @ <https://pro.nitrous.io/> to give a try at anytime. But if you sign up now, they will give you a $5 free credit. However, compared to their current offering, in this new "Pro" version, there is no free plan available for now.

OK, and let's dive in and take a look at the new home page!

{% include image.html img="assets/nitrous/homepage.png" title="Homepage" %}

As you could see, they adopted a new color schema, which is relatively different from their [current website](https://www.nitrous.io/), which is pretty colorful. I got to say that I quite like the new look, since it's quite modern and clean.


Before we login and go the console, I want to talk about the new pricing plan, which is shown below.

{% include image.html img="assets/nitrous/pricing.png" title="Homepage" %}

I got to say this is slightly more expensive compared to their current pricing, which starts free with the most basic plan starting at $19.95 per month. But considering the fact that at this price you got a dedicated machine with 20GB SSD storage and 1G shared memory, in my point of view, this is worth the price. And usually I am only running one application per time, so 1G memory should be enough for me experimenting all kinds of stuff. 

And let's go to the console now.

{% include image.html img="assets/nitrous/console.png" title="Homepage" %}

Again, this is quite different from their current UI, and it seems that they abandoned the "BOX" concept used in their current production version, but adopt the "Workspace" concept, and you could have multiple containers inside one "Workspace".

The conceptual design and terminology used there are pretty much "docker style", so if you are familiar with docker, this will make you feel at home. 

Moving on and let's try create a new container.

Click on "NEW CONTAINER..." and we will get following options.

{% include image.html img="assets/nitrous/option.png" title="Homepage" %}

OK, so now we get a lot of choices! From fully prepared environment to customized Docker image. This is really cool. So if I want to get something quick I could just lunch a pre-configured stack, and if I want to fine tune every step I could just use my own docker file.

Lastly, the new desktop application looks like this

{% include image.html img="assets/nitrous/desktopapp.png" title="desktopapp" %}

It looks pretty neat but the Shell function is not working for now. 

So this is my first impression on Nitrous Pro Beta version for now, leave a comment below and let me know how do you think about it!