---
layout: post
title: "Move BitBucket Repository to Github"
date: 2014-05-22 09:10:55 -0500
comments: true
categories: 
---

Alternatively titled "Move Remote Git repository to a new Host". These instructions apply for any remote git hosts.

{% codeblock lang:bash %}

#What remotes am I currently configured for?
git remote -v

#Remove these remotes
git remote rm origin

#Set up new remote
git remote add origin https://github.com/kylethielk/DailyOje.git

#Push existing history to new remote
git push -u origin master

{% endcodeblock %}


_Sidenote: Referring to services like Github as 'remote hosts' is disingenuous since git is decentralized. All repositories are treated equal and our daily git routine is simply keeping all of them in sync. Referring to them in this manner can help clarify which copy we are referring to._