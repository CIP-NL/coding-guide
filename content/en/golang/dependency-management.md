---
title: Dependency Management
linktitle: Dependency Management
description: "Dependency Management, go get, dep and vgo"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [dependency, dependency-management]
draft: false
menu:
  docs:
    parent: "golang"
    weight: 40
weight: 40
sections_weight: 40
aliases: [/golang/]
toc: false
---

## Packaging
A Go package always consists of all go files in a single directory. A repository
may contain multiple packages. Go has it's own package manager. The standard way to 
obtain packages is:

```commandline
go get url/mypackage
```

However `go get` does not support versioning.  Thus we use dep as our package manager. 
{{% note %}}
As of writing this, `vgo` is still in active development. Once it reaches a stable version
we will switch to `vgo`.
{{% /note %}}


Dep will create lock files and supports vendoring.

Download dep [here](https://github.com/golang/dep).

Make sure to let dep vendor your project to ensure reproducible builds.

## Libraries
The go standard library is broad and very optimized, so use this 
as much as possible, while refraining from using too many dependencies. 
Although, don't reinvent the wheel either. If you find an open source package 
of acceptable quality, use it.
 
