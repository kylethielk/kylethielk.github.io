---
author: Kyle Thielk
comments: true
date: 2013-06-03 06:50:29+00:00
layout: post
slug: twitter-findtofollow-update
title: Twitter-FindToFollow Update
wordpress_id: 570
categories:
- Projects
---

My recently released Open Source Twitter tool [FindToFollow](https://github.com/kylethielk/twitter-findtofollow/) has received some much needed updates and can now be used to automatically follow users that have been filtered. The follows occur successively with a randomly generated interval (with a set minimum and maximum) between each follow.

On the roadmap:



	
  1. Filter currently followed users based on a set of criteria so that we can unfollow them automatically for example: "Unfollow all users who have not followed me back in less than three days since I've followed them". Similar to what ManageFlitter provides. We however will have more information as we know when users were followed and unfollowed, information ManageFlitter does not have.

	
  2. Statistics about daily/weekly/monthly follower and friend counts.




![Automatically filter potential twitter users.](/media/images/twitter-findtofollow-screenshot.png "Twitter FindToFollow main filter screen.")

![FindToFollow automatically follow selected users.](/media/images/twitter-findtofollow-screenshot-2.png "FindToFollow automatically follow selected users.")
