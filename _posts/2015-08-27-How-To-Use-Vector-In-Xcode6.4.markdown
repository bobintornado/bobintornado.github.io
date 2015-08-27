---
layout: post
comments: true
title:  "How To Use Vector In Xcode 6.4"
date:   2015-08-27 19:24:36
categories: Development
---

Exporting the same image into three different sizes for your iPhone application is a really troublesome process, and this post aims to solve this problem by showing you how to use a single vector in Xcode 6.4.

#Step 1

Export the vector file into PDF format from Sketch.

{% include image.html img="assets/how_to_use_vector_in_xcode/step1.png" title="step1" %}

#Step 2

Go to your Assets.xcassets in Xcode and add new image set. 

#Step 3 

Select the image 

{% include image.html img="assets/how_to_use_vector_in_xcode/step3.png" title="step3" %}

#Step 4 

Go to inspector tab

{% include image.html img="assets/how_to_use_vector_in_xcode/step4.png" title="step4" %}

#Step 5

Change scale factors to `Single Vector`

{% include image.html img="assets/how_to_use_vector_in_xcode/step5.png" title="step5" %}

#Step 6

Drag and drop the exported PDF file into the Universal slot. 

{% include image.html img="assets/how_to_use_vector_in_xcode/step6.png" title="step6" %}

Enjoy.