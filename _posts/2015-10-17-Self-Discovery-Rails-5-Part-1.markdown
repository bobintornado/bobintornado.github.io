---
layout: post
comments: true
title:  "Self Discovery Rails 5 Part 1"
date:   2015-10-17 21:24:36
categories: Development
---

Disclaimer: Rails 5 is currently under heavy and active development, and everything written in this blog post is subject to change(changing is the only constant). And this post mainly serves as my self help study notes.

I decided to find out the difference between an API only Rails 5 project and a normal Rails 4.2.4 project.

So I generated two new Rails projects and looked into Gemfile first.

Firstly, in the 4.2.4 version Gemfile, we have `uglifier`, `coffee-rails`, `jquery-rails`, `turbolinks`, and `web-console` installed by default; and in the Rails 5 API version Gemfile, these gems are removed as they are no longer needed.

Secondly, gem `jbuilder` is removed and currently replaced by `active_model_serializers`. There is a debate regarding which gem is a better default choice in Rails 5, and you could read more in this [pull request](https://github.com/rails/rails/pull/19832). I have never seriously worked with these two gems, but I am more into the `active_model_serializers` approach by using serializers and adapters, instead of defining a "static" JSON template file in the jbuilder way, but that is only my personal preference for now.

Thirdly, gem `sdoc` is removed in Rails 5 API version Gemfile and currently there is no replacement, but we do have a few gem choices regarding API document generation on Github. And README.rdoc is also replaced by README.md in the root directory accordingly.

Fourthly, gem `sprockets` has been added into the Gemfile in Rails 5 Gemfile, so it is officially not a core feature of Rails anymore. 

Fifthly, gem `rack-cors` is added into the Gemfile in Rails 5 API version to handle cross-origin AJAX stuff, but it's by default commented out.

Sixthly, gems `rack`, `arel` and `tzinfo-data` are added in the Rails 5 API only version Gemfile as well.

Secondly I took a look into the app directory.

In the Rails 5 API only project, directory `assets`, `helpers`, and `views` are removed and a new directory `jobs` is added.

And the `ApplicationController` is now inherited from `ActionController::API` instead of `ActionController::Base`.

Thirdly I took at `config/application.rb` file. 

These lines are added into the file. 

{% highlight ruby %}
# Only loads a smaller set of middleware suitable for API only apps.
# Middleware like session, flash, cookies can be added back manually.
# Skip views, helpers and assets when generating a new resource.
config.api_only = true
{% endhighlight %}

Ok! Great to know!

That's all for part 1.