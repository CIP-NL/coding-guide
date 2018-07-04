---
title: CD
linktitle: CD
description: Continuous Delivery
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [workflow]
keywords: [ci,circleCI,travicCI]
authors: [Karel L. Kubat]
menu:
  docs:
    parent: "workflow"
    weight: 40
weight: 40
sections_weight: 40
draft: false
aliases: [/cd/]
toc: true
---

CD is closely tied to CI. The problem is: 

*How to get your code to the servers that will actually run it, and how do you push new code
on a daily basis.*

## Difference between CI and CD
CI is the method to integrate new code and test this with automated builds. CD concerns
itself with the next step: if the tests pass, push the artifacts (binaries, docker images, etc...) 
to the next environment. 


## Pipeline
A pipeline describes how new code reaches production. As an example I will describe the user backend
pipeline of the BlockStart fund.

1. Code is worked on in the development branch. CI is used to verify each merge.
2. Development is merged with master at the end of each day or new feature. CI runs a set of tests again.
3. Heroku builds a new app in the staging environment, which is validated by checking if other services
   depending on it still work.
4. If nothing breaks, the staging app is pushed to production. 

## Environments
Environments refer to different things depending on context (duh). Here we are not talking 
about python virtual environments, but different builds. 

#### Staging
Staging environment is an almost exact mimic of production environment. Using automated tests
helps in predicting the effects of a new feature on the existing ones, but cannot prevent
failure when multiple apps rely on a single one being updated. Staging is where we can
observe their behaviour.

#### Production
Production environment is the actual in use environment, the stuff that makes us money.
Breaking this is bad and will result in you having to pay for the beer on friday.



