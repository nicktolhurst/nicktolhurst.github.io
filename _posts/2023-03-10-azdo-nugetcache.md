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

In short, caching is a transient _higher_ speed data storage layer. Data is stored int this layer for faster retrieval than from it's original source.

In Azure DevOps Pipelines, there is the Cache@2 task. This enables us to store files / directories on the DevOps organisations cache layer, which can then be retrieved in subsequent runs. The goal to make subsequent runs faster, by not having to reproduce those files / directories.

Good use cases for Caching in Azure DevOps Pipelines are dependencies.

> *Caching isn't always the answer.* In some cases, it may not be economical to cache. Such as when the restore from cache takes longer than the alternative. 

### How to cache

In it's simplest form, all caching requires is a `cache key` and a `path`. The `key` is used to determine whether or not your cache should be invalidated or restored. The `path` is the directory which you want to store and retrieve.

#### Cache Key

The cache key is made up of strings, file paths, or file patterns, separated by `|`. The contents of files matched are hashed to produce a dynamic cache key with the idea being that the key can be unique enough to invalidate only when necessary.

A good example of a key for NPM dependencies, would be to use the `packages-lock.json` file. The only time we want this cache to invalidate is when we change a package dependency on our project. This, and only this, would cause a change to our `packages-lock.json` file. We may also want to use the string `npm` to identify the tool we are using, and the operating system built-in variable, to ensure matrix-strategy cross platform builds are supported.

## [warning]The given cache key has changed in its resolved value between restore and save steps;
<https://dotnetthoughts.net/improving-angular-ci-build-time-using-azure-devops/>

How to set up caching for npm packages?

{% highlight YAML %}

- task: Cache@2
  displayName: Cache node_modules (Assessments)
  inputs:
    key: 'npm | "$(Agent.OS)" | $(Build.SourcesDirectory)\package-lock.json'
    path: $(Build.SourcesDirectory)\node_modules
    cacheHitVar: CACHE_HIT

- task: Npm@1
  displayName: Install Node Modules
  inputs:
    command: 'install'
    workingDir: $(Build.SourcesDirectory)
  condition: ne(variables.CACHE_HIT, 'true')

- task: Npm@1
  displayName: Run NPM Build
  inputs:
    command: 'custom'
    workingDir: '$(Build.SourcesDirectory)
    customCommand: 'run build'

{% endhighlight %}
