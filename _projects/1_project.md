---
layout: page
title: lean ci in azure 
description: A project I'm working on to find any possible way to speed up Azure Pipelines.
img: assets/img/projects/water-line.jpg
importance: 1
category: work
---

As a way to deepen my understanding of Azure DevOps Pipelines, I decided to take a journey to over-optimize (is that a thing?) the performance of a frustratingly slow CI pipeline.

To give a little bit of a background, the CI pipeline I am working with builds a monolithic code-base. The tech stack is a mix of .NET Framework, .NET Core, .NET 6, and VueJS. It builds, tests, packages and publishes deployment packages to a NuGet feed for deployment.

I intend to disassemble every part of this pipeline, in an effort to deep dive into pipeline optimization. This project is intended to be a dump of all the things I learn along the way.