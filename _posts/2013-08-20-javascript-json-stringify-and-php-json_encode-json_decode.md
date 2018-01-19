---
layout: post
status: publish
published: true
title: JavaScript JSON.stringify and PHP json_encode json_decode
wordpress_id: 250
wordpress_url: http://lisalu.vm/?p=250
date: '2013-08-20 21:43:42 +1000'
date_gmt: '2013-08-20 11:43:42 +1000'
tags: javascript php
comments: []
---
I used Ajax call quite a lot recently and thought that knowing how to process JavaScript array and PHP arrays as JSON string is really handy.
So I'd like to note it down for future reference.

The following is my example (ignore the variable and class names which are just examples):

```javascript
function trackQuestions(){
     qStatus.length = 0;
     $('.map-content ul').each(function(){
          var label = $(this).attr('data-label');
          if(label != 'ny' && label != 'base'){
               qStatus.push(label);
          }
     }); // qStatus = ["q1", "q2", "q3", "q4"];
   
     $.ajax({
        type: "POST",
        url: url+"/processing.php",
        data: {qStatus: JSON.stringify(qStatus)},// JSON.stringify(qStatus) = '["q1", "q2", "q3", "q4"]'
        success: function(data) {
             console.log(data);// data = ["q1", "q2", "q3", "q4"];
        }
    });
     
}
```


```php
$qStatus = json_decode(stripslashes($_POST['qStatus']), true);
```

Some explanations of JSON.stringify(), json_decode() and json_encode():

[JavaScript JSON.stringify](http://msdn.microsoft.com/en-us/library/ie/cc836459(v=vs.94).aspx)

In my example, the replacer is not defined, then the output will be a string list "q1, q2, q3, q4".


[PHP json_encode](http://php.net/manual/en/function.json-encode.php)

`string json_encode ( mixed $value [, int $options = 0 [, int $depth = 512 ]] )`

Returns a string containing the JSON representation of the value.

The value being encoded can be any type except a [resource](http://www.php.net/manual/en/language.types.resource.php).

This function only works with UTF-8 encoded data.


[PHP json_decode](http://php.net/manual/en/function.json-decode.php)

`mixed json_decode ( string $json [, bool $assoc = false [, int $depth = 512 [, int $options = 0 ]]] )`

Converts the json format value into a PHP variable. 

Values *true*, *false* and *null* (case-insensitive) are returned as *TRUE*, *FALSE* and *NULL* respectively. 
NULL is returned if the *json* cannot be decoded or if the encoded data is deeper than the recursion limit.

- the name and value must be enclosed in double quotes
- single quotes are not valid
- trailing commas are not allowed
