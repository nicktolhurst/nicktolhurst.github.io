---
layout: post
title: an introduction to faster cicd
date: 11-03-2023
description: >
  A blog series documenting my learnings as I journey towards massively optimising #AzureDevOps Pipelines.
tags: azure-devops azure cicd performance
categories: azure-devops
---

{% include figure.html path="assets/img/ci-cd-logo.png" class="img-fluid no-border hero rounded z-depth-1" %}

### Why?

Nowadays, software development relies heavily on CI / CD pipelines to build, test, package and deploy your products. The productivity of your development team directly depends on the speed of your CI / CD pipelines. With that in mind, it's clear how important it is to periodically benchmark and invest in CI / CD improvements.

But how much effort should we invest?

Optimising CI / CD for performance eventually comes with diminishing returns. The amount of time and effort investigating, benchmarking, proving and discarding will ultimately become economically impractical. Fortunately, I am not concerned with the ROI of my efforts in this blog series. In this blog series, I'm here to waste my own time in an effort to produce the most gains.

I will share the output of my journey here, hoping it will help others optimise their pipelines with fewer mistakes and headaches than I experience.

### First, some context

I frequently work with a large monolithic code base. Over 50 developers are working on this repository. There are hundreds of projects that each require building, testing, packaging, and publishing. Once published, the deployment of these projects is handled outside of #AzureDevOps.

Despite the size of the code base, productivity isn't terrible. The developers work together in unison to deliver features and enhancements into production many times per day. But there are inefficiencies within the design, process and implementation of the CI / CD pipeline that, once removed, would accelerate the productivity of our teams.

Today is day 0. Today is where we will start this journey, and our current software development life cycle (SDLC) benchmarks are as follows:

- Feature Development takes ~ 55m to deploy to a testing environment.
- PR / Code Review takes ~ 55m to run automated build validation tasks.
- Deployment takes ~ 55m to build and create a production channel deployment.

Yes - these times are suspiciously similar. We will cover that in a future post.

{% include posts_in_category_cards.html %}
