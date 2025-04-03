---
title: Tracing custom TCP
description: Bpftrace for NVIDIA XLIO
categories:
- General

feature_image: "jakaś mapka pokazująca ścieżkę przechodzącą przez labirynt"
---

## Background
In this post I would like to show the ways I utilize bpftrace during my research on problems contained within XLIO library, NVIDIAs custom TCP stack. 

Bpftrace has been a real life-saver when trying work on different libraries or kernel drivers. The way I see it is akin to a query language that can be used, to know something about the program You are working on. An analogy could to be found in SQL where, if I have a question: 
> What is the weekly distribution of chocolate sales in the month of March?

then an analyst (or chatbot) can construct a query for a specific database schema that would have to SELECT some data from the database tables, maybe perform some joining and finally produce the result.

A similarly phrased question that could be asked about a system like TCP, which bpftrace allows answering is:
> What are all the code paths that instatiate a certain class?


<!-- I hope that the practical examples have been insightful or helpful so that You may want to check eBPF. -->
