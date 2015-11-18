---
layout: post
comments: true
title:  "Practicing Git"
date:   2015-11-19 01:24:36
categories: Git
---

I'd like to share something about git I learned the hard way in the wild.

1. Branch often, *complete* often, merge often. Branch a lot, they are really cheap. So in order to follow this, you need to figure out a way to split your feature into a reasonable amount of branches(think sequentially), complete feature step by step(branch by branch, pull request by pull request), then merge them back into your team's main development branch. This greatly helps with code review, because there will be less codes to review each time. This will leads to more code reviews, and more code reviews is a good thing, because it helps your teams understand each other better, particularly each other's subtle coding styles(in other words, how do your teammates think).

2. Write good commit messages. I mean, really good commit messages. They could be really useful. So how do you do it? Read [this](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message) and [this](http://chris.beams.io/posts/git-commit/). Good(short, concise, summarized) commit messages could literally serve as some sort of documentations.

3. Once you decided to write good commit messages, you will immediately understand how useful `git rebase -i` is. So basically this tool allows to you checkout the branch, do a bunch of hacks, write all kinds random commit messages for yourself, and in the last tidy up as if you wrote all these codes in one breath like a poem. For example, I literally committed stuff for almost every single change, and then once I have done my hacks, I just connect the dots backwards(much easier than forwards) by doing rebase, where I could tight up my commits and commit messages. In this way, I could clearly and logically convey what I have done. The good thing about this approach is, regardless in what random orders I put together my codes, I could always rearrange them in a easy-to-follow way, so other other people have a much easier time going through my commits and understand what the heck have I done.

4. Assuming you have done above, then you will discover `git log` is really useful, try using `git log -p`, `git log --pretty=oneline` and `git log --stat`. You will feel these commands suddenly make sense, a lot of sense.

## Note
Special thanks to Ted and Wei-liang for sharing their thoughts and knowledge with me.
