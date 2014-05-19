---
author: Kyle Thielk
comments: true
date: 2013-01-19 21:41:36+00:00
layout: post
slug: full-page-two-column-layout-without-tables
title: Full Page Two Column Layout without Tables
wordpress_id: 192
categories:
- Design
---

_Disclaimer: The end goal would be trivially easy with tables. That is not the point of this post._

The goal is simple. Produce a simple two column layout using only CSS and no tables. Each column has to extend to the end of the page regardless of the height of the content it contains. Much trickier than it sounds.

<!-- more -->


### The Basics


{% codeblock lang:html %}
<div style="float: left; width: 250px;">Left Column...</div>
<div style="float: left; width: 500px;">Long text....</div>
{% endcodeblock %}

Produces:

![](/media/images/html-columns1.gif "Columns 1")

Obviously no good. The height of the columns do not match. A simple wrapping DIV with an explicit height can fix that


### 




### Force Columns to Same Height


{% codeblock lang:html %}
<div style="height: 400px;">
<div style="float: left; width: 250px; height: 100%; background-color: #8bb8ef;">
  Left Column...
</div>
<div style="float: left; width: 300px; height: 100%; background-color: #d1d1d1;">
  Long text....
</div>
</div>
{% endcodeblock %}

Which then yields:

![](/media/images/html-columns2.gif)

Both columns are the same height! However this requires us to know the exact height of our content. Lets keep trying.


### 




### Full Page Columns?


Setting the height of body, html, and div to 100% might work:

{% codeblock lang:html %}
<style>
html,body,div{     height: 100%; }
</style>
<div style="float: left; width: 250px; background-color: #8bb8ef;">
  Left Column...
</div>
<div style="float: left; width: 300px; background-color: #d1d1d1;">
  Long text....
</div>
{% endcodeblock %}

If we explicitly set the height of html,body and div to 100%, we can then get the following:

![](/media/images/html-columns3.gif)

Awesome! Both columns extend to the end of the page and we didn't have to specify any explicit heights. However once your content extends past the browser's viewport, trouble arises.

![](/media/images/html-columns4.gif)


### The Solution


It turns out we have to take a slightly different and more complicated approach.

{% codeblock lang:html %}
<style>
html,body{
     height: 100%;
}
#wrap {
     background: #8BB8EF;
     float: left;
     min-height: 100%;
     position: relative;
    right: 200px;
    width: 400px;
}
#left-column{
    float: left;
    left: 200px;
    position: relative;
    width: 200px;
}
#right-column{
    float: left;
    left: 400px;
    position: relative;
    width: 600px;
}
</style>

<div id="wrap">
<div id="left-column">Left Column...</div>
<div id="right-column">Long text....</div>
</div>
{% endcodeblock %}

And tada! This gives us the desired result. Both Columns always occupy the entire height of the page, regardless of their content! There is a little bit of trickery going on here. If you notice the #left-column's background is actually coming from the #wrap container. By setting the wrap's position to relative and setting left: 200px, we can force it off the screen just enough to leave us with a 200px left column background.

Here is an illustration to explain a little better what is going on:

![](/media/images/html-columns51.gif)
