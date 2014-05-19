---
author: Kyle Thielk
comments: true
date: 2013-02-15 17:58:33+00:00
layout: post
slug: wordpress-plugin-development-using-media-library
title: 'Wordpress Plugin Development: Using Media Library'
wordpress_id: 287
categories:
- Projects
---

When porting [ jquery-refolio](https://github.com/kylethielk/jquery-refolio) over to wordpress one of the things I wanted to do was tie into wordpress' Media Library. This makes it extremely easy to re-use images already in wordpress and it would mean I didn't have to [re-invent the wheel](http://www.codinghorror.com/blog/2009/02/dont-reinvent-the-wheel-unless-you-plan-on-learning-more-about-wheels.html). Shockingly enough I couldn't find it documented anywhere in Wordpress' officials documentation and even a few quick googles came up empty.

I eventually stumbled upon the answer and I was suprised at how easy it was. Hopefully this will help someone in the future trying to do the same thing.

To launch the media manager from javascript:

{% codeblock lang:javascript %}
   //launch media library popup
   tb_show('Choose Portfolio Image', 'media-upload.php?type=image&TB_iframe=true');
{% endcodeblock %}

You must then register a callback as such (the name is important and must be registered in the global namespace):

{% codeblock lang:javascript %}

/**
* @param html Will be passed the html for the selected image.
*/
window.send_to_editor = function (html)
{
   var imageUrl = jQuery('img', html).attr('src');

   //Hide media library popup.
   tb_remove();

};

{% endcodeblock %}

The callback will be invoked when the user has selected an image to insert. Here is a sample HTML snippet passed back to the callback function:

{% codeblock lang:html %}
<a href="http://www.kylethielk.com/blog/?attachment_id=971" rel="attachment wp-att-971">
  <img src="http://www.kylethielk.com/blog/wp-content/uploads/2013/02/big-image2.jpg" alt="" width="484" height="317" class="alignnone size-full wp-image-971" />
</a>
{% endcodeblock %}

Good luck!
