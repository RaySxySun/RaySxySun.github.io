---
layout: post
title: FrontEnd - CSS Equidistant
date: 2016-04-27
weather: sunny
categories: Design 
tags: [FrontEnd]
description: 
---

> [Design Patterns Index](http://raysxysun.github.io/categories/#FrontEnd)

# CSS: Equidistant

<img src="{{ site.url }}/assets/img/2016-04-27-FrontEnd/equidistant.png" alt="{{ page.title }} at {{ site.title }}">

> Flexbox Justification

it's the best solution

	<div class="container">
	  <div style="float: left;"></div>
	  <div></div>
	  <div></div>
	  <div style="float: right; margin-right:10px;"></div>
	</div>​

	.container {
	  display: flex;
	  justify-content: space-between;
	}

> [FrontEnd](http://raysxysun.github.io/categories/#FrontEnd)
