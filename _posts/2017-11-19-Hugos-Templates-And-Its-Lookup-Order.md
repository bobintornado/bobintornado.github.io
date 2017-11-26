---
layout: post
comments: true
title:  "Hugo's Templates And Its Lookup Order"
date:   2017-11-19 03:24:36
categories: Development
---

So recently I built a simple [portfolio website](https://toniachen.com/) with [Hugo](http://gohugo.io/). ([Hugo content repo](https://github.com/bobintornado/toniachen-hugo).) 

It was a pretty fun experience and I found Hugo is indeed blazing fast. On top of that, I think it has some sensible defaults out of the box, even though it's not documented very well.

While I am learning Hugo I found the templating part a bit hard to figure out, so I decided to write a summary of it.

First, template lookup is just simple cascading, babe. Meaning for every single content it will look up its template in a hierarchy order and pick the first one to render. If the content couldn't find any template you will get a 404 error when visiting the intended URL.

Second, the master template (the bread) should be put at `layout/_default/baseof.html` with `main` block to park the beef later. (It's just normal Golang `HTML/template` stuff)

{% highlight go %}

{% raw %}

{{ block "main" . }} {{ end }}

{% endraw %}

{% endhighlight %}

In all other templates, you could use this master template by using 


{% highlight go %}

{% raw %}

{{ define "main" }}

your beef

{{ end }}

{% endraw %}

{% endhighlight %}


Third, there are two major templates, namely `list` and `single`. `list` is like the index page, and maps to path `/`; `single` is the like the detail page, and maps to `/:some-content-based-URL`.

For example, in the example site I built, there is a `UI` section in the nav bar, which maps to `/uis/` path. Under Hugo's default configurations, the folder named `uis` under `content` folder will be used. And upon visiting `/uis/` path, Hugo will look up the template in the order talked about in this [documentation page](https://gohugo.io/templates/section-templates/#section-template-lookup-order). 

There is a catch here is that under version `v0.30.2`, if you have an empty folder, the `list` template will not be rendered automatically. You must put a `_index.md` file under the folder, even the content of file `_index.md` could be empty.

The catch is useful when adding static pages. Eg, when adding `About`, `FAQ`, or `Privacy` pages.

To add such a page, first create a folder with the desired name, for example, `about`, then creating an empty `_index.md` file under the folder. After that just dump whatever static HTML into the `list.html` template. 

In the `about` case, dump the content to `layout/about/list.html`.

That's all about the templating, Happy generating. :)
