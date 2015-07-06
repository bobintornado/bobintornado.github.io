---
layout: post
comments: true
title:  "How I Install Jekyll On OS X Yosemite"
date:   2015-02-20 19:24:36
categories: Development
---

`Update`: This post has some minor issues here and there and is a bit outdated now. Please don't follow step by step. The general idea here is to use `rbenv` as ruby development environment controller so that we could avoid using `sudo` when install jekyll.

Well, I try to follow <http://jekyllrb.com/docs/installation/> and do my own installation, but I did run into some small troubles and I'd like to share some of them.

Firstly, I type the following command line

~~~
$ gem install Jekyll
~~~

And I run into this error

>ERROR:  While executing gem ... (Gem::FilePermissionError)  
>You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory

Mmm. OK, so I don't the permission. So I Google around, and it turns out that the Ruby 2.0.0 is shipped with my OS X system, and it's advised that I didn't touch it.

(Read more about it at <http://stackoverflow.com/questions/14607193/installing-gem-or-updating-rubygems-fails-with-permissions-error>)

Of course I could always do 

~~~
$ sudo gem install Jekyll
~~~

But I think using too much `sudo` is a bad idea, and I don't want to mess up with my system, so I decide to follow the path of multiple versions installation.

After reading the Stack Overflow question above, I think **rbenv** sounds like a good solution, so I decide to go ahead and give it a try.

Since I use Homebrew, so I just do 

~~~
$ brew update
$ brew install rbenv
~~~

Afterwards I need to add eval `$(rbenv init -)` to my profile as stated in the caveats.

Since I am using Zsh so I just go ahead and edit my Zsh profile at `~/.zshrc`, and adding this one line into my profile. 

After adding this one last line now I could finally use rbenv. So I get a list of all versions with following command.

~~~
$ rbenv install -l
~~~

And after going through the list, I decide to install 2.2.0 version, as that looks a good, updated and stable version. 

~~~
$ rbenv install 2.2.0
~~~

Since I'm lazy, so I decide to put 2.2.0 as global, so I create the `~/.rbenv/version` file and type in `2.2.0` to set my global Ruby version to 2.2.0.

And now I could finally install Jekyll on my Yosemite with

~~~
$ gem install Jekyll
~~~

I Skip the part about pre-realse and also "Optional Extras” since I don't really need them for now. 

After one last step, which is to put $PATH correctly by adding the following line into the bottom of my `~./zshrc` file.

~~~
export PATH=~/.rbenv:$PATH
~~~

So now let's do

~~~
$ gem install jekyll
$ jekyll new my-awesome-site
$ cd my-awesome-site
~/my-awesome-site $ jekyll serve
# => Now browse to http://localhost:4000
~~~~

and start having fun!