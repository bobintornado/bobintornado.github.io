---
layout: post
comments: true
title:  "Hugo's Templates And Its Lookup Order"
date:   2017-11-19 03:24:36
categories: Development
---

So recently I built a simple [portfolio website](https://toniachen.com/) with [Hugo](http://gohugo.io/). ([Hugo content repo](https://github.com/bobintornado/toniachen-hugo).) 

It was a pretty fun experience and I found Hugo is indeed blazing fast and quite flexible. And Most importantly, I think it has some sensible defaults out of box.

While I am learning to use Hugo I found the templating part a bit hard to figure out, so I decided to write a summary on it.

First, template lookup is just simple cascading, babe. Meaning for every single content it will look up its tempalte in a hierarchy order and pick the first one to render. If the content couldn't find any tempalte you will get a 404 error when visiting the url.

Second, the master tempalte (the breads) should be put at `layout/_default/baseof.html` with `main` block to park the beef later. (It's just normal Golang `html/template` package stuff)

```
{{ block "main" . }} {{ end }}
```

In all other templates, you you could use this master tempalte by using 

```
{{ define "main" }}

your beef

{{ end }}
```

Third, there are two major tempaltes, `list` and `single`. `list` is like the index page, and maps to path `/`; `single` is the like the detail page, and maps to `/:some-content-based-url`.

For example, in the example site I built, there is a `UI` section in the nav bar, which maps to `/uis/` path. Under Hugo's default configurations, all one needs to do is to create a folder named `uis` under `content` folder. And upon visiting `/uis/` path, Hugo will look up the template in the order talked about in this [documentation page](https://gohugo.io/templates/section-templates/#section-template-lookup-order). 

There is a catch here is that under version `v0.30.2`, if you have an empty folder, the `list` template will not be rendered automatically. You must put an `_index.md` file under the folder, even the content is of the `_index.md` is empty.

The catch is useful when adding for static pages. Eg, when adding `About`, `FAQ`, or `Privacy`.

To add such a page, first create a folder with the desired name, for example `about`, then creating a empty `_index.md` file under the folder. The last step to dump whatever static html into the `list.html` tempalte. In the `about` case, dump the content to `layout/about/list.html`.

That's all, and happy generating!
