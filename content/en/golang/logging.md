---
title: Logging
linktitle: Logging
description: "Logging in Golang"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [style, logging, monitoring]
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

Logging is what ensures Ruud wakes up at night, and what allows Karel to go to bed
worry-free.

## Logging
Logging is the second most important part of writing code, after automated tests but more 
important than monitoring. In all honestly, The Go standard library does not handle logging 
very elegantly. You can read up some more on it over [here](https://logmatic.io/blog/our-guide-to-a-golang-logs-world/)

Logging does come with a perofrmance hit, but don't worry too much about it. Early optimisation
is the root of all evil.

## Logrus
We prefer using [logrus](https://github.com/sirupsen/logrus) for logging purposes. It is an extension on the standard log interface,
and allows us to add some convenient [hooks](https://github.com/CIP-NL/logrus-hooks) to 
outside services.

## Configuring logging
Logging should be configurable by the config file, environment variables, and especially
by passing a flag. That makes debugging a lot easier. Viper has the watchconfig option.
Allowing the app to change the log level without a restart is especially powerful.

