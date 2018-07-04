---
title: CI
linktitle: CI
description: Continuous Integration
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
aliases: [/ci/]
toc: true
---

Continous Integration is not just the practise of adding TravisCI to your 
repository and displaying a nice badge with a green build status. 

## Frequent Integration vs CI
Frequent integration is necessary to ensure a project does not end up in 
"integration hell", where different PRs are so different and branches so far 
apart that merging might break many things. In FI, the integration is mainly done
manually, best used for big new features.

In CI, automated tests ensure that new code does not break the build and work properly.
The aim is to be able to merge code multiple times a day. Usually the tests are performed
by a third party, such as TravisCI or CircleCI.

## CI services.
CircleCI and TravisCI are two services that will monitor a github or bitbucket repository,
and on pushes, pull requests, and merges, clone the repository and run a test script.

The test script needs to be provided by the repository owner. Below you find two examples for 
CircleCI and TravisCI

#### CircleCI config.yml
```
# version 2 indicates the version of CircleCI. version 1 is no longer supported 
version: 2
jobs:
  build:
    working_directory: ~/CIP
    
    # CircleCI uses docker images to create an environment, which it pulls from dockerHub
    docker:
    
      # An image by CircleCi, with lots of python tools built in.  
      - image: circleci/python:3.6.4
        environment:
          PIPENV_VENV_IN_PROJECT: true
          #  Python decouple will read .env file, and use those if environment variables are not set.
          #  Precedences: env var, .env file, default values.
          PIPENV_DONT_LOAD_ENV: true
          DATABASE_URL: postgresql://root@localhost/circle_test?sslmode=disable
          CIRCLECI: true
          TESTING: true
          
          # We log some testdata and factory seeds
          LOG_FILE_LOCATION: "$CIRCLE_ARTIFACTS"
      
      # Simple postgres container
      - image: circleci/postgres:9.6.2
        environment:
          POSTGRES_USER: root
          POSTGRES_DB: circle_test

    steps:
      - checkout
      - run: sudo chown -R circleci:circleci /usr/local/bin
      - run: sudo chown -R circleci:circleci /usr/local/lib/python3.6/site-packages
      
      # We cache dependencies.
      - restore_cache:
          key: deps9-{{ .Branch }}-{{ checksum "Pipfile.lock" }}
      
      # pipenv is the preferred python packaging manager.
      - run:
          command: |
            sudo pip install pipenv
            pipenv install
      
      # save our new cache
      - save_cache:
          key: deps9-{{ .Branch }}-{{ checksum "Pipfile.lock" }}
          paths:
            - ".venv"
            - "/usr/local/bin"
            - "/usr/local/lib/python3.6/site-packages"
      
      - run:
          command: |
            pipenv run python manage.py test
      
      # Will store the logfile.
      - store_artifacts:
          path: "$CIRCLE_ARTIFACTS"
```

#### TravisCI config
```
# This is a weird way of telling Travis to use the fast container-based test
# runner instead of the slow VM-based runner.
sudo: false

language: go

# Only the last two Go releases are supported by the Go team with security
# updates. Any older versions be considered deprecated. Don't bother testing
# with them.
go:
  - 1.9.x
  - 1.x

# Only clone the most recent commit.
git:
  depth: 1

# Skip the install step. Don't `go get` dependencies. Only build with the code
# in vendor/
install: true

# Don't email me the results of the test runs.
notifications:
  email: false

# Anything in before_script that returns a nonzero exit code will flunk the
# build and immediately stop. It's sorta like having set -e enabled in bash.
# Make sure golangci-lint is vendored by running
#   dep ensure -add github.com/golangci/golangci-lint/cmd/golangci-lint
# ...and adding this to your Gopkg.toml file.
#   required = ["github.com/golangci/golangci-lint/cmd/golangci-lint"]
before_script:
  - go install ./vendor/github.com/golangci/golangci-lint/cmd/golangci-lint

# script always runs to completion (set +e). If we have linter issues AND a
# failing test, we want to see both. Configure golangci-lint with a
# .golangci.yml file at the top level of your repo.
script:
  - golangci-lint run       # run a bunch of code checkers/linters in parallel
  - go test -v -race ./...  # Run all the tests with the race detector enabled
```

## Build minutes and caching.
Not only do we pay per build minute, waiting for long builds also just sucks.
In the circleCI config.yml we are caching dependencies. This makes each build cheaper and 
ensures that we can do more builds per day.

TravisCI is free for open sourced projects. In general we don't open source our code, except for
some libraries. CircleCI is the preferred option for CI (simply because I know it a bit 
better). 


## Summary
Basically ensure that each projects CI configuration is working properly.





