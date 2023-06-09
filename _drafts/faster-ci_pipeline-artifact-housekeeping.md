---
layout: post
title:  azure devops - reduce pipeline artifacts
date:   
description: >
  Using PowerShell to rapidly & recursively stage artifacts for publishing.
tags: Azure DevOps Pipelines CICD PowerShell
categories: azure-devops
---

### What is the problem?

I have a large monolithic code-base. There's 100s of projects. Each project has a handful of test projects. These test projects output directories are packaged up published as pipeline artifacts, to be used in the test stages. Collating these files - 59050, or 3gb! - takes ~3m 30s, publishing them takes ~1m 15s, and, on the later test stages, downloading them again takes ~1m.


### My approach

 - Find out if we really need everything? If we do, do we need everything every time?
 - Are there any inefficiencies in how we collate our build output?


#### Caching NPM dependencies - `node_modules`

{% highlight PowerShell %}

param(
    $TargetPath,
    $BuildConfiguration = 'Release',
    $ArtifactsToPublish
)

$tempDir = New-Item -ItemType Directory "$TargetPath" -Force

$overallPerformance = Measure-Command {

    $ArtifactsToPublish | ForEach-Object {
        $artifact = $_
        $parameters = $_.Parameters
        $tempDirFullName = $tempDir.FullName

        $results = Get-ChildItem @parameters 

        Write-Host "Found $($results.Count) $($artifact.Name) directories using filter '$($parameters.Path)'." # -NoNewline

        $results | Foreach-Object -ThrottleLimit 10 -Parallel {

            $destinationPath = "$($using:tempDirFullName)\$($using:artifact.Name)\$($_.BaseName)"

            $destination = New-Item -ItemType Directory -Path $destinationPath  -Force -Verbose

            $performance = Measure-Command {
                $_ | Get-ChildItem -Filter "bin/$($using:BuildConfiguration)/**" -ErrorAction SilentlyContinue | Copy-Item -Destination  $destination -Force -Recurse -Verbose
            }

            Write-Host "Completed copy of '$($_.BaseName)' to: '$destinationPath\bin\$($using:BuildConfiguration)\*' ($($performance.TotalMilliseconds) ms)"
        }
    }
}

Write-Host "`r`nCopy completed in $($overallPerformance.TotalMilliseconds) ms." -ForegroundColor Green


{% endhighlight %}
