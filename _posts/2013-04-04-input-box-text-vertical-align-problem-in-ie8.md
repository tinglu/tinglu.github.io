---
layout: post
status: publish
published: true
title: Input box text vertical align problem in IE8
wordpress_id: 207
wordpress_url: http://lisalu.vm/?p=207
date: '2013-04-04 20:32:18 +1100'
date_gmt: '2013-04-04 10:32:18 +1100'
tags: IE css
comments:
- id: 48
  author: David
  author_email: dd60137@hotmail.com
  author_url: ''
  date: '2015-10-21 04:02:00 +1100'
  date_gmt: '2015-10-20 18:02:00 +1100'
  content: Thanks! Just the information I was looking for.
---
In IE7 or IE8 you will find there are problems with CSS settings which work very well in other browsers.
I've encountered the input box text vertical alignment issues recently in IE.

As the image shown below, the text of the input box vertically aligned in the center automatically in Chrome, Firefox (and maybe IE9+).

Chrome:

![text_in_chrome]({{ site.url }}/assets/css/line-height/chrome.png)

Firefox:
![text_in_firefox]({{ site.url }}/assets/css/line-height/firefox.png)

However, it is aligned to the bottom in IE9 or below:
![text_in_ie]({{ site.url }}/assets/css/line-height/ie.png)

Assume the input box in this example has a fixed height:
```html
<input type="text" value="Search" />
```
```css
.text {
    height: 50px;
    font-size: 12px;
}
```

To make the text vertically aligned in the center in IE browsers, you could simply set the **line-height CSS property** to the same height as of the element. In this example, it is line-height: 50px.
```css
.text {
    height: 50px;
    line-height: 50px;
    font-size: 12px;
}
```

Find other answers of [Incorrect vertical alignment in ie8](http://stackoverflow.com/questions/3893256/incorrect-vertical-alignment-in-ie8) and [How to vertical align text on input box for ie](http://stackoverflow.com/questions/6412696/how-to-vertical-align-text-on-this-input-box-for-ie) on Stack Overflow.
