---
title: Automated Testing
linktitle: Automated Testing
description: Language agnostic automatic testing
date: 2016-11-01
publishdate: 2016-11-01
lastmod: 2018-01-02
categories: [workflow]
authors: ["Karel L. Kubat"]
keywords: [testing,unit-testing,automated testing]
menu:
  docs:
    parent: "workflow"
    weight: 30
weight: 30
sections_weight: 30
draft: false
aliases: [/automated-testing/]
toc: true
---

Automated testing and Test Driven Development (TDD) are important strategies while designing and writing
code, to minimize the amount of bugs, but also ensure that after a few months of coding, we are not stuck with
a monolithic monstrosity where we cannot change any fundamental features out of fear of breaking a core functionality.

## Testing principles
Instead of loading your website and manually navigating to different urls to verify if the pages are loading properly, one
writes a script to do all of this work. The same goes for a complicated backend feature, a code library or the google search
engine.

Instead of writing some python or bash script to test your C++ or Go code, you use the internal testrunner and write tests
in that specific language. A simple example in Go:


``` 
## math.go
func Add(first int) int {
    return first + 2
}


## math_test.go
// t *testing.T is associated with Golang's internal testrunner
func test_Add(t *testing.T) {
    
    testcases := map[int]int{
        1: 3,
        2: 4
        -1000000: -999998
        1000000000: 1000000002
    }
    
    for case, result := range testcases {
        res := Add(case)
        if res !=  result {
            t.Errorf("Add did function correctly: Expected: %s, Actual %s", result, res)
        }
    }
```

Now if you were to change some internal business logic to Add, you'd run test_Add to verify that it still works as expected.
In a large project, you might run thousands of tests before committing a change. Doing this by hand is simply not possible.

## Unit testing

test_Add is a textbook example of a unit test. It tests a single function, and verifies it works over expected
ranges (1, 2), and extremes (-1000000, -1000000000). There are no other dependencies that might fail, unrelated to the 
actual function, such as APIs and databases.

You should be able to run a unit test completely locally, without the need for an internet connection or other dependencies.

## Mocking
Your functions or classes might rely on a dependency, such as a DB connection or API client. To write unit tests for these,
use mock constructs to mock the DB or API:

``` 
## db_functions.py
def query_db(db: DB, queryset: dict) -> dict:
    """returns a formatted resultset""" 
    
    raw_results = db.query(queryset)
    formatted_results = raw_results.keys.upper()
    return formatted_results
    
## mock.py
class MockDb:
    def query(queryset: dict) -> dict:
        return {"EXPECTED": "result"}
        


## tests.py
class Test_query_db(TestCase):
    def setup():
        self.db = MockDb()
       
    def test_query_db_1():
        from db_functions.py import query_db
        queryset = {"columns": ["results"]}
        expected = {"EXPECTED": "result"}
        
        self.assertEqual(expected, query_db(queryset))  
```

We program the mock object to return certain results, regardless of input. This test is quite useless, but as your app
starts relying on more dependencies, mocks become very handy.

When writing classes, interfaces or structs, also generate mock objects, so others using your code can access these mocks.
The writer of the code is responsible for its testability.

## Integration Testing
An integration test does rely on dependencies. We trigger our integration tests by 
supplying the environment variable INTEGRATION=true when running tests.

Our python test would then be:  

``` 
## tests.py
class Test_query_db(TestCase):
    def setup():
        if config("INTEGRATION", cast=bool):
            self.db = DB.connect(config("DB_URL"))
        else:
            self.db = MockDb()
       
    def test_query_db_1():
        from db_functions.py import query_db
        queryset = {"columns": ["results"]}
        expected = {"EXPECTED": "result"}
        
        self.assertEqual(expected, query_db(queryset))
```

When testing locally, you don't want to set all of these configurations, such as DB_URL, each time
you want to run an integration test. Instead use a .env (dotenv) file, which you load every time you run the tests/code.

The .env file is not committed. Add it to your gitignore. Make sure to document all required env variables and expected values.

## Writing Testable Code
It should become apparent that when writing code, you keep the testability in mind. If it seems impossible to write a unit tests,
you have a clear indication that something is wrong with the code structure.

Some people find it even easier to first write a test, and then a function or method that passes that test. This is TDD.
Others however write a bunch of code, and spend all afternoon testing it. Regardless of preference, ensure that each commit includes tests and mocks.
If you've fixed a bug, add a unit test replicating that bug, and verifying that the new implementation has fixed that bug. 

## Links

* [Nice blog](https://medium.com/@lazlojuly/how-to-get-started-with-unit-testing-part-1-7f490bbf560a)