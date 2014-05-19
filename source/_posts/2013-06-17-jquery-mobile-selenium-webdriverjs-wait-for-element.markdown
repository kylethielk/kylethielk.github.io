---
author: Kyle Thielk
comments: true
date: 2013-06-17 02:42:28+00:00
layout: post
slug: jquery-mobile-selenium-webdriverjs-wait-for-element
title: Waiting For Element with jQuery Mobile & WebDriverJS
wordpress_id: 587
categories:
- General
---

Selenium's webdriver API provides a convenient way to wait until an element is visible and ready, a common thing to do when testing AJAXy web apps.

In Java: 
{% codeblock lang:java %}
By byId= By.id("idOfElementWaitingFor");
wait.until( ExpectedConditions.visibilityOfElementLocated(byId) );
{% endcodeblock %}

[WebDriverJS ](https://code.google.com/p/selenium/wiki/WebDriverJs) is a fantastic library of JavaScript bindings for Selenium's webserver that allows us to write our automated tests in javascript. It just makes sense (and feels correct) to write automated tests in javascript when your webapp is entirely javascript as well.

Unfortunately I encountered an issue almost immediately waiting for an element to visible and available when using jQuery Mobile, and surprisingly could find little help in the usual places (read: google).  This is how I solved it.

{% codeblock lang:javascript %}
waitFor: function(elementId, timeout)
{
	timeout = timeout || 5000;
	var _this = this;
	var deferred = this.webDriver.promise.defer();

	this.driver
		.wait(function()
		{
			return _this.driver.isElementPresent(_this.webDriver.By.id(elementId));
		}, timeout)
		.then(function()
		{
			return _this.driver.wait(function()
			{
				return this.driver
                                .findElement(_this.webDriver.By.id(elementId))
                                .isDisplayed();
			}, timeout);
		})
		.then(deferred.fulfill);
	return deferred.promise;
}
{% endcodeblock %}

There are three tricks here.

1. WebDriverJS is 100% non-blocking, we thus have to use the provided Promises construct, adding just a little bit of complexity.
2. jQuery Mobile, particularly in regards to paging, heavily utilizes hiding/showing elements so we have to ensure that the element exists AND that it is visible. 
3. WebDriverJS' findElement() method will throw an error that is not suppressible. So we have to be careful using it when we fully expect an element not exist for a while.

This method helps us solve all three. It first waits for the element to exist in the DOM, it then waits for the element to be visible and then it fulfills our promise. For example Selenium correctly complains when we try to click an element that exists, but is not visible. 

For further clarification here is a sample usage:

{% codeblock lang:javascript %}

waitFor('my-action-btn', 10000)
    .then(function () 
    {
        //...perform action on my-action-btn   
    });


{% endcodeblock %}

Just a note if the element never becomes available and visible, the error thrown by WebDriverJs' wait method will be propagated through.
