---
layout: post
title: Database - relational DB Background - Big O notation
date: 2016-05-26
weather: cloudy
categories: DB 
tags: [DB]
description: 
---

> [Design Patterns Category](http://raysxysun.github.io/categories/#DB)


# Background Knowledge

- Big O notation: The time complexity is used to see how long an algorithm will take for a given amount of data.
	- The O(1) or constant complexity stays constant (otherwise it wouldn’t be called constant complexity).
	- The O(log(n)) stays low even with billions of data.
	- The worst complexity is the O(n2) where the number of operations quickly explodes.
	- The two other complexities are quickly increasing.

<img src="{{ site.url }}/assets/img/2016-05-26-DB/BigO.png" alt="{{ page.title }} at {{ site.title }}">

- Example: An algorithm needs to process 1 000 000 elements
	- An O(1) algorithm will cost you 1 operation (ex. hash table)
	- An O(log(n)) algorithm will cost you 14 operations (ex. well-balanced tree)
	- An O(n) algorithm will cost you 1 000 000 operations (ex. an array)
	- An O(n*log(n)) algorithm will cost you 14 000 000 operations (ex. best sorting algorithms)
	- An O(n2) algorithm will cost you 1 000 000 000 000 operations (ex. bad sorting algorithm)

> The time complexity is often the worst case scenario.


> [Design Patterns Category](http://raysxysun.github.io/categories/#DB)



