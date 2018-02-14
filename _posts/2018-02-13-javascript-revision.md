---
layout: post
title: JavaScript Revision
date: 2018-02-13 18:27 +1100
tags: javascript
---

Some JavaScript dot points of this and that (details refer to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)).

## Basics

There are 7 data types as defined in ECMAScript standard:

- 6 primitives (**primitive values are immutable**):

	* Boolean
	* Null
	* Undefined
	* Number
	* String
	* Symbol (new in ECMAScript 6)
	
- Object (In JavaScript, objects can be seen as a collection of properties)


| Primitive Types     | Primitive Wrapper Object | typeof                         | Value     | 
| ------------------- | ------------------------ | ------------------------------ |------------------- | 
| Boolean             | Boolean                  |typeof boolean = "undefined"    | true, false |
| Number              | Number                   | typeof number = "undefined"    | -(2^53 -1) to 2^53 -1 and 3 symbolic values: +Infinity, -Infinity, and NaN |
| String              | String                   | typeof string = "undefined"    | a set of "elements" of 16-bit unsigned integer values |
| Symbol              | Symbol                   | typeof symbol = "undefined"    | a unique and immutable; may be used as the key of an Object property |
| Null                |                          | **typeof null = "object"**     | null |
| Undefined           |                          | typeof undefined = "undefined" | undefined |

## Map vs. Set

**Map** objects hold key-value pairs. 
Key equality is based on the "SameValueZero" algorithm: NaN is considered the same as NaN (even though NaN !== NaN) 
and all other values are considered equal according to the semantics of the === operator. 

**Set** objects store **unique** values of any type.
NaN and undefined can also be stored in a Set. 
NaN is considered the same as NaN (even though NaN !== NaN).

For both map and set, -0 and +0 are considered equal in the current ECMAScript specification but were not so in earlier drafts.

## Map vs. Object

* The keys of an Object are Strings and Symbols, whereas they can be any value for a Map, including functions, objects, and any primitive.
* You can get the size of a Map easily with the size property, while the number of properties in an Object must be determined manually.
* A Map is an iterable and can thus be directly iterated, whereas iterating over an Object requires obtaining its keys in some fashion and iterating over them.
* An Object has a prototype, so there are default keys in the map that could collide with your keys if you're not careful. As of ES5 this can be bypassed by using map = Object.create(null), but this is seldom done.
* A Map may perform better in scenarios involving frequent addition and removal of key pairs.