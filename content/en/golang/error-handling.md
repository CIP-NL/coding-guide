---
title: Error Handling
linktitle: Error Handling
description: "Error Handling in Golang"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [golang]
keywords: [errors, style, coding]
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

Error handling is usually 90% of the logic in a full business app, so better spend some time 
studying up on it.

## Exceptions vs Errors
Golang does not have exceptions, instead a pointer to a struct filling the error interface is returned.
This means that every function that may create an error, will return one, and must be explicitly handled.
In go we often observe the following structure: 

```go
someVar, err := myFunc()
if err != nil {
	handleErr(err)
}
```

This is the idomatic way to handle these errors. Don't try to handle them somewhere else, deal 
with them directly. Panic should only be used by top level functions, or when the error 
could only result from a programming error.

## Error interface
In go-error-boilerplate you can see the following Error interface

```go
// Error should be a return parameter from a function instead of error.
type Error interface {
	error
	Code() string
	Kind() Kind
	Public() (string, bool)
	Retry() bool
}

// Kind defines a certain class of error, and should remain constant per package to allow for error handling.
type Kind uint8

// An enumeration. Make sure the first one is None, all others may be changed depending on package.
const (
	None          Kind = iota
	Other              // Unclassified error. This value is not printed in the error message.
	Invalid            // Invalid operation for this type of item.
	Permission         // Permission denied.
	IO                 // External I/O error such as network failure.
	Exist              // Item already exists.
	NotExist           // Item does not exist.
	IsDir              // Item is a directory.
	NotDir             // Item is not a directory.
	NotEmpty           // Directory not empty.
	Private            // Information withheld.
	Internal           // Internal error or inconsistency.
	CannotDecrypt      // No wrapped key for user with read access.
	Transient          // A transient error.
	BrokenLink         // Link target does not exist.
)
```

This is an expanded error interface. Code and Kind may be used for assertions, and should be considered
part of the stable API of an app. Public is a human readable string used for debugging
by operators.

Retry is a boolean, indicating if the action causing the error may be retried. This is less 
relevant within an app, and more relevant to outside consumers of an API e.g.

{{% note %}}
protip: Create a script using [corgi](https://github.com/DrakeW/corgi) to automatically
create an error file in your project. I use:
```bash
wget https://raw.githubusercontent.com/CIP-NL/go-error-boilerplate/master/error.go -O error.go
```
Don't forget to edit the package name.
{{% /note %}}

## Casting errors
If a library or other API has their own error interface, make sure to cast it to our own 
interface before passing it along. Especially APIs may have many different error codes. 
Ensure that your package recognises and handles each of these errors.
