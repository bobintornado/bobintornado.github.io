---
layout: post
comments: true
title:  "I Don't Really Know HTML"
date:   2015-10-17 20:24:36
categories: Development
---

I thought I really know HTML. I thought I know it so well that I could write HTML in my dream -- come on, you call HTML codes?

But recently I realized how naive I am, cause one of my friends asked me to build an infinite load feature, so when user scrolls down something to a certain point, the app should automatically load more content.

And I had no ideas about how could I determine that user is going to reach the "end" of some lists, but after some Googling I stumble upon some DOM properties, which are `scrollheight`, `scrollTop` and `clientHeight`. 

Jaw-dropping.

When does HTML DOM has those properties?! And how come I have never heard of them before?

Ok, never mind, let me check them out. 

So it turns out `scrollHeight` represents the total height of the scrollable area, which is somehow fixed; `clientHeight` is the height of visible area, which is fixed at the point of scrolling (it may change when user adjust the size of the visible area); and `scrollTop` is the height of "already scrolled" area, which is dynamic. 

To better illustrate the idea, I draw two boxes, where the bigger box is the full content at full height, and the smaller box is the visible area. 

{% include image.html img="assets/idontknowhtml/drawing.png" title="drawing" %}

Okay, talking is cheap so I also made a small JSFiddle to further demonstrate the idea in codes. Try scrolling the orange box and watch those numbers change. 

<iframe width="100%" height="350" src="//jsfiddle.net/bobintornado/h5p76m5s/1/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

So now I could just monitor the how far user is from the "bottom" of the scrollable area (distance, which equals scrollheight - scrollTop - clientHeight), and trigger the load more AJAX request when distance reaches certain limit, say 300 pixels or something like that.

Hope you had fun reading and now know exactly what are `scrollheight`, `scrollTop` and `clientHeight`!

For more details, check out

* <https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollHeight>
* <https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollTop>
* <https://developer.mozilla.org/en-US/docs/Web/API/Element/clientHeight>