---
layout: post
title:  azure devops - caching dependencies
date:   2023-03-10
description: >
    Using caching in your pipeline is a great way to speed up your pipeline's execution time.
tags: Azure DevOps Pipelines CICD
categories: azure-devops
---

### Introduction

What is caching? Caching is typically a high speed data storage layer used to serve up data faster than retrieving it from it's original source. 

How does caching in azure devops help?

When does caching in azure devops not help?

What's the difference between caching and using artifacts?

In it's simplest form, all caching needs to work is a `cache key` and a `path`. The key is used to determine whether or not your cache should be invalidated or restored.

How to set up caching for npm packages?

{% highlight YAML %}

- job: build-node-app
  displayName: Build Node App
  steps:

  - task: UseNode@1
    inputs:
      version: 'latest'

  - task: Npm@1
    displayName: Install vueJs shared apps node modules
    condition: ne(variables.SHARED_NODE_MODULES_CACHE_HIT, 'true')
    inputs:
      command: 'install'
      workingDir: '$(Build.SourcesDirectory)\src\Mars\Mars\Scripts\vue-apps\shared'

  - task: Npm@1
    displayName: Build vueJs assessments app
    inputs:
      command: 'custom'
      workingDir: '$(Build.SourcesDirectory)
      customCommand: 'run build'
      
{% endhighlight %}
