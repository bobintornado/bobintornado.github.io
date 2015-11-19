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

3. Once you decided to write good commit messages, you will immediately understand how useful `git rebase -i` is. So basically this tool allows you to checkout the branch, do a bunch of hacks, write all kinds random commit messages for yourself, but in the last tidy them up as if you wrote all these codes in one breath like a poem. However, you shouldn't rely too much on this technique, and goes too wild. A general strategy I took is hacking everything first, then committed chuck by chuck, then lastly do a bit rebasing. As git is not a tool to document the process about how did you put together your codes, but a way to communicate with your follow peers.

4. Assuming you have done above, then you will discover `git log` is really useful, try using `git log -p`, `git log --pretty=oneline` and `git log --stat`. You will feel these commands suddenly make sense, a lot of sense.

## Note
Special thanks to Ted and Wei-liang for sharing their thoughts and knowledge with me.
