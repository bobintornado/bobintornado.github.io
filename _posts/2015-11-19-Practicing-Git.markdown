---
layout: post
comments: true
title:  "Practicing Git"
date:   2015-11-19 01:24:36
categories: Git
---

I'd like to share something about git I learned the hard way in the wild.

1. Branch often, *complete* often, merge often. Branch a lot, they are really cheap. So in order to do follow this, you need to figure out a way to split your feature into a reasonable amount of branches(think sequentially), complete feature step by step, then merge them back into your team's main development branch. This greatly helps with code review, because there will be lesser codes to review each time. This will leads to more code reviews, and more code reviews is a good thing, because it helps your teams understand each other better, particularly each one's subtle coding styles(in other words, how do your teammates think).

2. Write good commit messages. I mean, really good commit messages. They could be really useful. So how do you do it? Read [this](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message) and [this](http://chris.beams.io/posts/git-commit/). Good(short, concise, summarized) commit messages could literally serve as some sort of documentations.

3. Once you decided to write good commit messages, you will immediately understand how useful `git rebase -i` is. So basically this tool allows to you checkout the branch, do a bunch of hacks, write all kinds random commit messages for yourself. For example, I literally committed stuff for almost every single changes. Then once you have done your hacks, you could connect the dots backwards by doing rebase, where you could tight up your commits and commit messages, which could clearly and logically convey what you have done. In this way, regardless in what random orders you put together you codes, you could always rearrange them in a easy-to-follow way, so other people have a much easier time going through your commits and understand what you are doing here.

4. Assuming you have done above, then you will discover `git log` is really useful, try using `git log -p`, `git log --pretty=oneline` and `git log --stat`. You will feel these commands suddenly make sense, a lot of sense.


## Note
Special thanks to Ted and Wei-liang for sharing their thoughts and knowledge with me.
