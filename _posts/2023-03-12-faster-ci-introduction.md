---
layout: post
title: faster ci - an introduction
date: 12-03-2023
description: >
  A blog series documenting my learnings as I journey towards massively optimising #AzureDevOps Pipelines.
tags: azure-devops azure cicd performance
categories: azure-devops
---

Nowadays, software development relies heavily on CI / CD pipelines to build, test, package and deploy your products. The productivity of your development team directly depends on the speed of your CI / CD pipelines. With that in mind, it's clear how important it is to periodically benchmark and invest in CI / CD improvements.

Is your CI / CD pipeline optimised perfectly? Unlikely. Is perfectly optimised the end goal? Also, unlikely. Optimising CI / CD for performance eventually comes with diminishing returns. The amount of time and effort investigating, benchmarking, proving and discarding will eventually become economically impractical.

The economics of my efforts in this blog series is not something I am concerned with. In this blog series, I will waste as much time as I want to spend discovering ways to optimise my CI / CD pipelines.

The output of my journey will be shared here, in the hope that others can optimise their own efforts to speed up their own CI / CD pipelines.

{% for post in tag[faster-ci] %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
