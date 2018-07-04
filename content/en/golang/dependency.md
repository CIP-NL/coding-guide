---
title: Dependencies
linktitle: Dependencies
description: "Dependency Injection"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [dependency, dependency injection]
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

Here we describe handling dependencies within a your golang code. For package management,
check package management

## Dependencies
A dependency is some service or object on which your class/function/app relies on. This might 
be an instance of a database connection, API client or simply a struct with associated methods.

Integrating these dependencies in your program can be done in multiple ways. Some strategies are:

#### func init()
You might construct all of your dependencies in the init func (which is always run before main).
The downside is that this introduces a global state in your program, which makes debugging more difficult.
Also hot reloads are not possible.

###### example:
```go
var db DB.Conn

func init() {
	db, err := DB.open(viper.GetString("DATABASE_URL"))
}
```

#### Self instantiation
An object might create it's own dependencies within it's own init function. This makes mocking the
object a lot more difficult though.

###### example:
```go
type MyStruct struct {
	db DB.Conn
	url string
}

func (m *MyStruct) init() {
	m.db, err := DB.open(m.url)
}

This is an impractical solution, because the testability of MyStruct is quite bad.
```

#### Dependency injection
 
To make testing easier and allowing for easier restarting and switching out of components, we use a form of dependency injection.
 Read this [blog](http://adnaan.badr.in/blog/2017/07/15/exploring-dependency-injection-in-go/) to familiarize yourself.


###### example:
```go
type defaultConfig struct {
	DB.Conn
	api.Client
}

type MyService struct {
	defaultConfig
}


func NewMyService(c defaultConfig, options ... func(*config)) MyService{} {
    for _, option := range options {
    option(&defaultConfig)
  }
}
```

Note that we pass a default configuration, and then adjust that by passing variadic functions. Those functions
might change the "mode" of the app.

