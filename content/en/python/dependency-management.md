---
title: Dependency Management
linktitle: Dependency Management
description: "Dependency Management, pip vs pipenv"
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [python]
keywords: [dependency, dependency-management]
draft: false
menu:
  docs:
    parent: "python"
    weight: 40
    identifier: Python-Dependency-Management

weight: 40
sections_weight: 40
aliases: [/python/]
toc: false
---

## Packaging
A python package is a module defined by a setup.py and built into a distribution. This distribution
contains metadata, package dependencies and version. Most package are uploaded to PyPI, the PYthon Package
Index.

Creating your own package is quite easy. Below is a samply configuration for the setup.py.

```python
from setuptools import setup


def readme():
    with open('README.md') as f:
        return f.read()

setup(
    name='dpos-deputy',
    version='0.0.35',
    packages=[''],
    long_description=readme(),
    url='https://github.com/BlockHub/dpos-deputy',
    license='MIT',
    author='Charles',
    author_email='karel@blockhub.nl',
    description='CLI for managing dpos delegates',
    install_requires=[
        'Click', 'Arky==1.3.1', 'dpostools', 'pid',
        'psycopg2-binary', 'raven', 'simple-crypt', 'pycrypto',
        'markdown'
    ],
    py_modules=['deputy'],
    entry_points='''
        [console_scripts]
        deputy=deputy:main
    ''',
    include_package_data=True,
)
```

Now running `setup.py sdist` will generate a tarball, which can
be uploaded using [twine](https://github.com/pypa/twine) to PyPI.

Uploading to PyPI means anyone can access your code.

## Downloading packages.
Usually when downloading a python package, you'll run `pip install package`.
This usually works fine. However now you are getting the most recent upload to 
PyPI. It is better to specify a version of the package, and putting
this in you requirements.txt

#### requirements.txt
```
cryptography==2.2.2
dateutils==0.6.6
dj-database-url==0.5.0
Django==2.0.4
```

Still, this does not always lead to reproducible builds, as the code
may be changed without updating the version. (although PyPI tries to 
prevent this). A better method is using [pipenv](https://docs.pipenv.org/)
to manage all of your packages. Pipenv uses the file signatures to 
ensure loaded packages are exactly as specified. It will also manage
you virtual environments for you, so there is less need in using Conda 
or other environment managers.




`