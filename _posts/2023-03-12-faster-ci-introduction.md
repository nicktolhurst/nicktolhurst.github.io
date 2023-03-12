---
layout: post
title: faster ci - an introduction
date: 12-03-2023
description: >
  A blog series documenting my learnings as I journey towards massively optimising #AzureDevOps Pipelines.
tags: azure-devops azure cicd performance
categories: azure-devops
---

{% include figure.html path="assets/img/ci-cd-logo.png" class="img-fluid no-border hero rounded z-depth-1" %}

Nowadays, software development relies heavily on CI / CD pipelines to build, test, package and deploy your products. The productivity of your development team directly depends on the speed of your CI / CD pipelines. With that in mind, it's clear how important it is to periodically benchmark and invest in CI / CD improvements.

But how much effort should you put into this task? Is your CI / CD pipeline optimised perfectly? Unlikely. Is perfectly optimised the end goal? Also, unlikely.

Optimising CI / CD for performance eventually comes with diminishing returns. The amount of time and effort investigating, benchmarking, proving and discarding will eventually become economically impractical. Fortunately, the ROI of my efforts in this blog series is not something I am concerned with. In this blog series, I will waste a foolish amount of time deep-diving and discovering ways to optimise my CI / CD pipelines, regardless of their worth.

The output of my journey will be shared here, in the hope that others can optimise their own efforts to speed up their own CI / CD pipelines, without having to make as many mistakes along the way.

#### The Series

{%- assign sorted_projects = site.posts | sort: "date" -%}

<!-- Generate cards for each post in this series. -->
<div class="container">
  <div class="row row-cols-2">
  {%- for post in sorted_projects limit:6 -%}
    {% for category in page.categories %}
      {% if post.categories contains category %}
        {% if post.url != page.url %}
          {% include posts_horizontal.html %}
        {% endif %}
      {%- endif %}
    {% endfor %}
  {%- endfor %}
  </div>
</div>
