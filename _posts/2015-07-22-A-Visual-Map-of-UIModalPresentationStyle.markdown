---
layout: post
comments: true
title:  "A Visual Map of UIModalPresentationStyle"
date:   2015-07-23 19:55:36
categories: others
---

Apple's iOS Developer library is great, yet I found sometimes a picture worths a thousand words. 

For example, for `modalPresentationStyle` under `UIViewController`, the explanation is really great and clear, but just not visual enough. I tried Googling around, but my luck is bad, so I decide to take some efforts and make a visual map of `UIModalPresentationStyle`, for myself and everyone else digging around. 

First thing first, under this enum, there are 9 different styles ready to be used, and they are 

{% highlight ruby %}
 
* FullScreen
* PageSheet
* FormSheet
* CurrentContext
* Custom
* OverFullScreen
* OverCurrentContext
* Popover
* None

{% endhighlight %}