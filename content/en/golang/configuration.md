---
title: Configuration
linktitle: Configuration
description: "Configuration of a Golang app."
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [style, config, configuration]
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

## Configuration
To configure an app, use [viper](https://github.com/spf13/viper). Viper supports watching configurations. The following types of configurations should be watched and changed within the app:

* Logging level
* Authentication tokens
* Rate limits
* Other behaviour

The following types of configurations should require an app restart:

* (Database) connection properties (DB_URL etc)

