---
title: Coding Environment
linktitle: Coding Environment
description: Setting up your local coding environment is a must, but there are some pitfalls.
date: 2017-01-02
publishdate: 2017-02-01
lastmod: 2017-03-09
categories: [workflow,fundamentals]
keywords: [source, organization, directories]
menu:
  docs:
    parent: "workflow"
    weight: 50
weight: 50
sections_weight: 50
draft: false
aliases: [/coding-env/]
toc: true
---

Build environments rely on a set project structure described in the config.yml. Setting up 
your local development environment correctly from the start will save you a lot of work.

## IDE
An integrated development environment aids in productivity. I prefer using the intelleji IDEs,
but VisualStudio is also a great tool. If you prefer vim, you might be a god or a monster.

Spend some time setting up your IDE, linters, integrations and getting to know it's git integration.
You will save a lot of time in the end. 

## Operating system
Ubuntu or OS X either are fine. Windows is too, but I will make fun of you. In general I prefer 
working on Linux, however by using docker you can mimic other environments perfectly.

## Directory structure
This might be just a golang specific example. Please update it if you encounter more language quirks.

CircleCI checks out a branch, and then start building. This means it creates a folder named 
repository_provider/organisation/repository. Meaning that the project trade-engine
is placed in github.com/CIP-NL/trade-engine. If your own working directory is src/projects/project, 
your internal imports within golang are:

``` 
import (
    "projects/projectA"
    "projects/projectB"
)
```

Your tests will run locally, however within circleCI they will fail, since the CircleCI directory structure
is: `src/github.com/project`

Instead for the circleCI build, the imports should look like this:

``` 
import (
    "github.com/projectA"
    "github.com/projectB"
)
```

Editing your imports means that the CircleCI build will pass. Local builds will now fail however.

The best way is to create a ~go/src/github.com/organisation and use that as your standard working directory
for any code that where the origin will be on github.com.




