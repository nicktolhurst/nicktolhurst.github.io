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

## Why go faster?

Nowadays, software development relies heavily on CI / CD pipelines to build, test, package and deploy your products. The productivity of your development team directly depends on the speed of your CI / CD pipelines. With that in mind, it's clear how important it is to periodically benchmark and invest in CI / CD improvements.

But how much effort should you put into this task? Is your CI / CD pipeline optimised perfectly? Unlikely. Is perfectly optimised the end goal? Also, unlikely.

Optimising CI / CD for performance eventually comes with diminishing returns. The amount of time and effort investigating, benchmarking, proving and discarding will eventually become economically impractical. Fortunately, the ROI of my efforts in this blog series is not something I am concerned with. In this blog series, I will waste a foolish amount of time deep-diving and discovering ways to optimise my CI / CD pipelines, regardless of their worth.

The output of my journey will be shared here, in the hope that others can optimise their own efforts to speed up their own CI / CD pipelines, without having to make as many mistakes along the way.

### But first...

Let's start with some context.

I frequently work with a large monolithic code-base. In fact, there are over 50 developers working on this repository. There are hundreds of projects that each require building, testing, packaging, and publishing. Once published, the deployment of these projects is handled outside of #AzureDevOps.

Despite the size of the code-base, productivity isn't terrible. The developers work together in unison to deliver features and enhancements into production many times per day. But there are inefficiencies within the design, process and implementation of the CI / CD pipeline that could accelerate the productivity of our teams.

This is day 0. This is where we are starting, and our current software development life cycle (SDLC) benchmarks are as follows:

- Feature Development takes ~ 55m to deploy to a testing environment.
- PR / Code Review takes ~ 55m to run automated build validation tasks.
- Deployment takes ~ 55m to build and create a production channel deployment.

Yes - these times are suspiciously similar. We will cover that in a future post.

{% include posts_in_category_cards.html %}
