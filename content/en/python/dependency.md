---
title: Dependencies
linktitle: Dependencies
description: "Dependency Injection"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [python]
keywords: [dependency, dependency injection]
identifier: "python"
draft: false
menu:
  docs:
    parent: "python"
    weight: 40
    identifier: Python-Dependencies

weight: 40
sections_weight: 40
aliases: [/python/]
toc: false
---

Here we describe handling dependencies within a your python code. For package management,
check package management

## Dependencies
A dependency is some service or object on which your class/function/app relies on. This might 
be an instance of a database connection, API client or simply a struct with associated methods.

Integrating these dependencies in your program can be done in multiple ways. Some strategies are:

#### \_\_init__()
You might construct all of your dependencies in the \_\_init__ of a class. This does introduce some problems.
Mocking becomes more difficult, and switching dependencies means you need to subclass the \_\_init__.

###### example:
```python
class ChildService(RegularService):
	def __init__():
		super().__init():
		self.db = psycopg.connect(CONNECTION_PARAM)
```

#### Dependency injection
A better solution is to use dependency injection. Instead of having the object create its
own dependencies, we provide it with all the object it needs:

###### example:
```python
class MyService:
	def __init__(db: DB, client: api.Client):
		self.db = db
		self.client = client
```

Now you can switch out a DB configuration at ease, or provide a mock object when unit testing.


