---
layout: post
title:  azure devops - caching dependencies
date:   2023-03-10
description: >
    Using caching in your pipeline is a great way to speed up your pipeline's execution time.
tags: Azure DevOps Pipelines CICD
categories: azure-devops
---

### What is caching?

In short, caching is typically a _higher_ speed data storage layer used to serve up data faster than retrieving it from it's original source.

In Azure DevOps Pipelines, we have available to us the Cache@2 task, which enables us to store build / download output, to be retrieved in subsequent runs faster than if we were to build and download them again.

*Caching isn't always the answer.* In some cases, it may not be economical to cache. Such as when the restore from cache takes longer than the alternative. 

### How to cache

In it's simplest form, all caching requires is a `cache key` and a `path`. The `key` is used to determine whether or not your cache should be invalidated or restored. The `path` is the directory which you want to store and retrieve.

#### Cache Key

The cache key is made up of strings, file paths, or file patterns, separated by `|`. The contents of files matched are hashed to produce a dynamic cache key with the idea being that the key can be unique enough to invalidate only when necessary.

A good example of a key for NPM dependencies, would be to use the `packages-lock.json` file. The only time we want this cache to invalidate is when we change a package dependency on our project. This, and only this, would cause a change to our `packages-lock.json` file. We may also want to use the string `npm` to identify the tool we are using, and the operating system built-in variable, to ensure matrix-strategy cross platform builds are supported.

## [warning]The given cache key has changed in its resolved value between restore and save steps;
<https://dotnetthoughts.net/improving-angular-ci-build-time-using-azure-devops/>

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
