---
title: Testing
linktitle: Testing
description: "Automated Testing in Python"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [python]
keywords: [testing]
draft: false
menu:
  docs:
    parent: "python"
    weight: 40
    identifier: Python-Testing

weight: 40
sections_weight: 40
aliases: [/golang/]
toc: false
---

## Testing
Testing is one of the most important aspects of writing code. We differentiate between three types of tests:

1. Unit tests
2. Integration tests
3. Staging 

Unit tests can be run completely standalone. This means these tests make no api calls, don't require a database and can be run locally.

Integration tests may require outside dependencies, such as a database, or outside api service (although those should be limited to stable apis only). They do not depend however on other services by us.

Staging is our final testing environment, where we mimic production environment as closely as possible. Here the app is tested for all of its functionalities as if it were live.

`go test` should run the unit tests, setting the env variable INTEGRATION=true should run integration tests. Use the flags package to configure the tests.

```go
func test_Something(t *testing.T) {
	if ! viper.GetBool("INTEGRATION") {
		t.Skip()
	}
}
```

Use the library [testify](https://github.com/stretchr/testify) to make assertions, and use [mock](https://github.com/golang/mock) to generate mock interfaces for all your services and dependencies to use in unit testing.

## Test locations
Tests should be present in the same directory as the actual project. simply append _test to the 
file being tested. This ensures that code and tests are always bundled.

``` 
github.com
-organisation
--project
--- main.go
--- main_test.go
--- xyz.go
--- xyz_test.go
```

A test in the test file follows the structure: test_nameOfFunctionBeingTested. 

## Testcases
In idiomatic go, a test map is used with multiple cases to verify if a function works.

###### example:
```go
// test_API verifies if the api returns correct responses.
func test_API(){
	testcase := map[Query][Response] {
		Query{"Cat"}: Response{"MIAAAAUUWW"}
		Query{"Dog"}: Response{"Wrrrufff"}
		Query{"Human"}: Response{"Blablabla"}
	}
	for query, response := range testcase {
		assertEqual(t, response, client.api.call(query))
	}
}
```

## Speed 
Make sure that your tests pass within 2 seconds. If running tests takes too long, you'll get 
annoyed and not run them often enough.


