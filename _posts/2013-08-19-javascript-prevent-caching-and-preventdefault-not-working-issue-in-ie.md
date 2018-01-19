---
layout: post
status: publish
published: true
title: JavaScript prevent caching and preventDefault() not working issue in IE
wordpress_id: 243
wordpress_url: http://lisalu.vm/?p=243
date: '2013-08-19 21:21:58 +1000'
date_gmt: '2013-08-19 11:21:58 +1000'
tags: javascript IE
comments: []
---
Weeks ago, I incorporated Bootstrap popover function into one of my Facebook apps. 
It worked fine in Chrome (blame myself I didn't do cross-browser testing in IE properly). 
This morning when it was supposed to go live we found that it didn't work in IE! 
While debugging, I noticed the js file was not the right version that was supposed to be using. 
The solution is simple and you must have already known this but it's worth noting it down: 
appending a timestamp to the JavaScript file should solve the problem 
(refer to [David Walsh's solution](http://davidwalsh.name/prevent-cache)).

Another problem occurred in this app was the JavaScript event `preventDefault()` supporting issue. 
IE doesn't support this so the compromise would be using `event.returnValue = false` 
(refer to [the answer on Stack Overflow](http://stackoverflow.com/questions/1000597/event-preventdefault-function-not-working-in-ie))
