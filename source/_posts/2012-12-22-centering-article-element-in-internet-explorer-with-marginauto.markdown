---
author: Kyle Thielk
comments: true
date: 2012-12-22 00:29:51+00:00
layout: post
slug: centering-article-element-in-internet-explorer-with-marginauto
title: Centering Article Element in Internet Explorer with margin:auto
wordpress_id: 88
categories:
- Design
---

Its amazing how many quirks internet explorer has when it comes to proper handlingof css. Another one to add to the list.

A common trick to center an element on the screen regardless of the user's resolution is to:

  1. Set the element's width	
  2. Define margin-left and margin-right to auto i.e
  
  {% codeblock lang:css %}margin: 0 auto;{% endcodeblock %}
or
  {% codeblock lang:css%}margin: 15px auto 5px auto;{% endcodeblock %}


Internet Explorer for the most part handles this correctly. ([See when it doesn't work properly](http://stackoverflow.com/questions/662341/using-margin-0-auto-in-internet-explorer-8))

Unfortunately this trick does not work on the [HTML5 article element](http://www.w3.org/TR/html-markup/article.html) in IE9 (not tested on other versions of Internet Explorer although I imagine they behave similar).

The workaround is to wrap your article element in a DIV or other equivalent block element.
