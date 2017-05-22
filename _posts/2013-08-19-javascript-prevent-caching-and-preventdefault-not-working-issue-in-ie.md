---
layout: post
status: publish
published: true
title: JavaScript prevent caching and preventDefault() not working issue in IE
excerpt: "Monday means a brand new week starts and if you're lucky that the weather
  is awesome then you'll tend to believe this week must be great. However, developers'
  life sometimes is more like riding a roller coster. I'm not exaggerating here (ok
  maybe a little bit) but this morning i just went through a nightmare which almost
  drove me mad. I don't want to behave mean but I really want to advocate to kill
  Internet Explorer by all means. It has brought me so much trouble ever since I started
  spending more time on front-end development. It can be claimed that I am not a master
  of JavaScript and CSS so that I always get stuck by some stupid problems. Every
  time when I found out where the problem was, I felt like swearing to IE.\r\n"
wordpress_id: 243
wordpress_url: http://lisalu.vm/?p=243
date: '2013-08-19 21:21:58 +1000'
date_gmt: '2013-08-19 11:21:58 +1000'
tags:
- JavaScript
- IE
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
