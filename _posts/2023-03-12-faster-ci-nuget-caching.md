---
layout: post
title: azure devops - dependency caching
date: 12-03-2023
description: >
    Using caching in your pipeline is a great way to speed up your pipeline's execution time.
tags: Azure DevOps Pipelines CICD
categories: azure-devops
---

{% include figure.html path="assets/img/nuget-cache.png" class="img-fluid no-border hero rounded z-depth-1" %}

### What is caching?

In short, caching is a transient high speed data storage layer, in which data is stored for faster retrieval than from it's original source.

For #AzureDevOps, this enables us to store our projects dependencies on pipelines cache layer, which can then be retrieved in subsequent runs. This can make subsequent runs, that have no change in dependencies, much faster.

In some cases, it may not be economical to cache as the retrieval from cache may take longer than downloading from the origin.

### How to cache

In it's simplest form, all caching requires is a `cache key` and a `path`. The `key` is used to determine whether or not your cache should be invalidated or restored. The `path` is the directory which you want to store and retrieve.

#### The cache key

The cache key is made up of strings, file paths, or file patterns, separated by `|`. The contents of files matched are hashed to produce a dynamic cache key with the idea being that the key will be unique enough to invalidate the cache only when necessary.

A good example of a key for NPM dependencies, would be to use a `packages-lock.json` file. We know that our `packages-lock.json` only changes when we change a dependency. This is perfect, as we can re-use the same dependencies for each build as long as we haven't changed them. If we do change them, the cache will be invalidated, and we will proceed to update the cache with the new dependencies.

A suitable cache key for NPM dependencies would be `npm | "$(Agent.OS)" | **/package-lock.json`. Including `"$(Agent.OS)"` gives us the ability to run cross-platform builds, where the package dependencies may differ due to OS differences.

#### Caching NPM dependencies - `node_modules`

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

### Gotchyas

> [warning]The given cache key has changed in its resolved value between restore and save steps;

Something has happened between the `Cache@2` step and the end of the job that has caused the dynamic hash key to change. Check that the tool versions that generated / updated the files used in the key are the same. For example, using `NuGet 5.1.0` to restore and generate `package.lock.json` files on your development machine won't match the build servers `package.lock.json` files, if the agent has `NuGet 6.0.0` installed.
