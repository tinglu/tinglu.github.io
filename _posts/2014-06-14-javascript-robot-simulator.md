---
layout: post
status: publish
published: true
title: JavaScript Robot Simulator (Updated)
excerpt: "<em>(Updated in July 2015)</em>\r\n\r\nThis is a JavaScript robot simulator
  using HTML5 Canvas that moves&nbsp;around a square table top.\r\n\r\nThe robot is
  free to roam the table, but must be prevented from falling to destruction. Any movement
  that would result in the robot falling from the table must be prevented.\r\n\r\n"
wordpress_id: 389
wordpress_url: http://lisalu.vm/?p=389
date: '2014-06-14 16:31:30 +1000'
date_gmt: '2014-06-14 06:31:30 +1000'
categories:
- CS
tags: []
comments: []
---
*Updated in July 2015*

This is a JavaScript robot simulator using HTML5 Canvas that moves&nbsp;around a square table top.

The robot is free to roam the table, but must be prevented from falling to destruction. 
Any movement that would result in the robot falling from the table must be prevented.

The application can read commands in following format:
```text
PLACE X,Y,F
MOVE
LEFT
RIGHT
REPORT
```

Here's the code in jsfiddle. Feel free to have a look and leave your comments here.

<iframe src="//jsfiddle.net/lisatinglu/hsqu6vz0/embedded/" width="100%" height="1000" frameborder="0" allowfullscreen="allowfullscreen"></iframe>
