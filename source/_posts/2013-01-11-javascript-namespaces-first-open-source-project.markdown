---
author: Kyle Thielk
comments: true
date: 2013-01-11 21:21:10+00:00
layout: post
slug: javascript-namespaces-first-open-source-project
title: Javascript Namespaces & First Open Source Project
wordpress_id: 132
categories:
- Projects
---

Direct Link to Github: [js-namespace](https://github.com/kylethielk/js-namespace)

One of the first issues any reasonable size javascript projects encounters is proper 'modularization' of code.  This becomes especially critical when you are


	
  * Working on a multi-person team.

	
  * Using many third party libraries.


The global namespace quickly becomes polluted and the chances of a collision become higher. The solution for this issue is menial in most programming languages. In Java there are packages, C# has namespaces, python has namespaces etc... Javascript has no out of the box language support for a similar concept. There are literally hundreds of different implementations available online. Searching for 'Javascript Namespaces' on any search engine will yield plenty of options.

The basic approach works but is tedious and requires that all parts of the 'namespace' be predefined:

{% codeblock lang:javascript %}
var Company = {};
Company.name = "Widget Company";
Company.greeting = function()
{
	return "Hello, we are" + foo.name;
}
{% endcodeblock %}

For example to create the namespace "Company.Organization.TeamA.Product.Core" one could:

{% codeblock lang:javascript %}
var Company = {};
Company.Organization = {};
Company.Organization.TeamA = {};
//...and so on
{% endcodeblock %}

If you later decide you want to add Company.Organization.TeamB you have to be extra careful not to override your existing declarations:

{% codeblock lang:javascript %}
var Company = Company || {};
Company.Organization = Company.Organization || {};
Company.Organization.TeamB = Company.Organization.TeamB || {};
{% endcodeblock %}

After exploring (and using) many of the different solutions available online I decided to write my own. Its simple, easy to use and is **my first open source project....ever**! I am of course [re-inventing the wheel](http://www.codinghorror.com/blog/2009/02/dont-reinvent-the-wheel-unless-you-plan-on-learning-more-about-wheels.html) but I wanted to something simple to start my open source journey with.

It is available under the MIT license and the source can be found on github under [js-namespace](https://github.com/kylethielk/js-namespace). Its usage is simple.

{% codeblock lang:javascript %}

JsNamespace("Company.Organization.TeamA.Product.Core").MyClass = {};

{% endcodeblock %}

All namespaces and their immediate children will also have a 'className' defined. I've found this to be invaluable when logging.

{% codeblock lang:javascript %}

JsNamespace("Company.Organization.TeamA.Product.Core").MyClass =
{
    myFunction: function()
    {
        console.log(this.className + ": A log message");
    }
};

$(document).ready(function()
{
    JsNamespace.buildClassNames();

    Company.Organization.TeamA.Product.Core.MyClass.myFunction();
});

{% endcodeblock %}

Looks like my [public accountability](http://www.kylethielk.com/blog/open-source-projects-and-github-as-a-resume/) is off to a good start.

![JS Namespace](/media/images/js-namespace.gif "JS Namespace")
