---
layout: post
title: "Wordpress to Octopress"
date: 2014-05-22 09:30:13 -0500
comments: true
categories: Tech
---

Self hosting wordpress is no fun. Even without any plugins installed there is a constant stream of security updates to worry about. Every plugin you do use is a huge gaping security hole waiting to happen. With each and every wordpress update theres a 50/50 chance it will break your theme and/or plugins. Ever tried updating a theme that you made custom changes to?

Using a hosted platform like wordpress.com or WPEngine.com relieves you of most of this burden, but there are still the [security exploits](http://www.cvedetails.com/vulnerability-list/vendor_id-2337/product_id-4096/Wordpress-Wordpress.html) constantly being found. It also costs an arm and a leg for a humble personal blog.

![My weekly reminder to update VPS and Wordpress Security](/media/images/VPS-Security-Reminder.png "My weekly reminder to update VPS and Wordpress Security")

[Octopress](http://octopress.org) with free hosting on [Github](https://www.github.com) is one of the better options. Hosting on Github gives you the benefit of their reliability and CDN (not to mention its free!). Using Octopress means a completely (almost) static site thats easy to maintain and quick to load. 

Starting an octopress blog from scratch is dead simple. [Follow the directions here](http://octopress.org/docs/). Converting from Wordpress is a little bit tricky, but still manageable. Use [ExitWP](https://github.com/thomasf/exitwp) to convert your posts to markdown. If you have comments convert them to Disqus and they should seamlessly transfer if the URL stays the same.

Following these two guides as well as Octopress' own documenation I was able to do the entire transfer in an afternoon.

[Migrating 40,000 Posts from Wordpress to Octopress](http://zhen.org/blog/migrating-40000-posts-from-wordpress-to-octopress/)

[Switching to the Octopress Blogging Engine](http://blog.pixelingene.com/2011/09/switching-to-the-octopress-blogging-engine/)

Good luck!
