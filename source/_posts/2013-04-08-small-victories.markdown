---
layout: post
title: "Small Victories"
date: 2013-04-08 21:58
comments: true
published: false
categories: 
- Optimizations
- Python
- ClusterPy
- RiSE
---

Lately I have been working in optimizing and parallelizing the ClusterPy library and at the moment, I can celebrate some 'Small Victories'.

So far I have been making small changes in places that have huge impact -functions that get called a lot and unnecessary function calling- and the timer now stops a bit earlier.