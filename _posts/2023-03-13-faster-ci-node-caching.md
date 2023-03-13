---
layout: post
title: node dependency caching
date: 13-03-2023
description: >
    Cache Node.JS dependencies between pipeline runs. 
tags: Azure DevOps Pipelines CICD
categories: azure-devops
---

{% include figure.html path="assets/img/npm.png" class="img-fluid no-border hero rounded z-depth-1" %}

### What is caching?

In short, caching is a transient high-speed data storage layer that stores data for faster retrieval than from the source.

For #AzureDevOps, this enables us to store our project's dependencies on the pipeline's cache layer and then retrieve them on subsequent runs. This additional task can make subsequent runs much faster.

In some cases, caching may not be economical, as the retrieval from a cache may take longer than downloading from the origin.

### How to use the `Cache@2` task?

The most basic implementation of `Cache@2` requires a `cache key` and a `path`. The `key` is used to generate a dynamic hash that determines whether or not there is a valid cache stored. The `path` is the directory you want to store and retrieve.

The cache key consists of strings, file paths, or file patterns separated by a pipe character (`|`).

Use strings as identifiers, a good example would be a tool name, tool version, or OS version. Think, "if I change my OS version, do my dependencies change?". If so, having the OS version referenced in the cache key is correct.

A file path (file pattern) produces a hash based on the file (collection of files), resulting in a unique key each time the contents of this file (these files) change. Think, "What changes only when I update my dependencies?". Use that file as a part of the cache key.

For example, we will look at the pipeline's NPM dependencies. In the pipeline, we require a specific Node version. This node version matches the installed tools of our developers. In this case, we are not concerned with the version of the tool. We will start our key with simply the tool `node`. Though not an immediate requirement, we likely want to build the application on Windows and Linux operating systems. Therefore, the cache key can extend to `node | "$(Agent.Os)"`. Finally, to determine whether or not the dependencies have changed, we will target a single `package-lock.json` file. We know the `package-lock.json` only updates when a Node dependency has changed. Our final cache key is `node | "$(Agent.OS)" | **/package-lock.json`.

> Gotchya: Keys with a period (`.`) in them are considered file paths. To avoid this interpretation, wrap those keys in double quotes (`"`), as we have the `"$(Agent.OS)"` segment in the example.

The `path` parameter value is the location of the Node dependencies. In our case, this is `$(Build.SourcesDirectory)\node_modules`.

Having `node_modules` pull from the cache is considerably quicker than downloading them using `npm`. Moreover, installing `node_modules` when they already exist is quicker than installing them into an empty directory. However, do we need to install them if we have retrieved them from the cache?

By setting the task parameter `cacheHitVar`, a build-scoped variable is available to us and can be used as a condition in subsequent tasks, allowing us to avoid the unnecessary re-install of `node_modules` if they already exist on disk. Therefore, we will set `cacheHitVar` to `NODE_CACHE_HIT` and add the condition `ne(variables.NODE_CACHE_HIT, 'true')` to the step that installs our dependencies.

#### Example

{% highlight YAML %}

- task: Cache@2
  displayName: Cache node_modules Directory
  inputs:
    key: 'npm | "$(Agent.OS)" | $(Build.SourcesDirectory)\package-lock.json'
    path: $(Build.SourcesDirectory)\node_modules
    cacheHitVar: NODE_CACHE_HIT

- task: Npm@1
  displayName: Install node_modules
  inputs:
    command: 'install'
    workingDir: $(Build.SourcesDirectory)
  condition: ne(variables.NODE_CACHE_HIT, 'true')

{% endhighlight %}

What took approximately 2 minutes and 40 seconds now takes 35 seconds on each subsequent run after a dependency change. In this example, we can apply this to 2 different Node applications, saving 3 minutes and 40 seconds.

There we have it! Our Node dependencies are cached, and a considerable saving from merely a few extra lines of code.

What could be next? Caching NuGet packages?
