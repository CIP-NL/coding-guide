---
title: Style Guide
linktitle: Style Guide
description: "General golang styleguide"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [style]
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

## Idiomatic golang.

Start out by reading on [idiomatic](https://dmitri.shuralyov.com/idiomatic-go) go. We only differ from these examples on the package naming. We do use the plural in naming directories as opposed to [this](https://dmitri.shuralyov.com/idiomatic-go#use-singular-form-for-collection-repo-folder-name).

Next all our code is formatted using gofmt, as opposed to formatting it ourselves. Don't waste time on indentation etc. just format it. The code is checked using golanci-lint. Install it as follows:

```commandline
go get github.com/golangci/golangci-lint/cmd/golangci-lint
```

This globally installs golangci. It checks for common style mistakes and more. Our CI also checks using golangci, so make sure to use it.

You run it by typing:

```commandline
golangci-lint run
```

## Project level directory structure
Golang does not have the concept of package level directories. Each directoy is a completely 
new package which can be imported independently by different projects. A package should be 
do one thing and one thing well. Don't create a package "utilities" and dump random functions 
in them.

## Dependency injection 
To make testing easier and allowing for easier restarting and switching out of components, we use a form of dependency injection. Read this [blog](http://adnaan.badr.in/blog/2017/07/15/exploring-dependency-injection-in-go/) to familiarize yourself.

The following pattern is used when creating a new service:

```go

func NewService(c defaultConfig, options ... func(*config)) MyService{} {
    for _, option := range options {
    option(&defaultConfig)
  }
}
```

Note that we pass a default configuration (which should return a reasonable workable app) and then adjust those configurations using different config options.

## main.go
In practice main.go should be structured as follows:

func init() should be used to only configure viper with the config file directory, possibly given by parsing a flag.

func main() should be occupied for 90 percent by instantiating the configuration from viper and building the app; no business logic should be present there.

The configuration object may contain a pointer to viper. This is convenient when watching different variables.

