[![Build Status](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio.svg?token=qf9Qoz1exW1V6BM8uvXA&branch=main)](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio)

# Travis CI ePortfolio

![Travis CI Logo](https://docs.travis-ci.com/images/TravisCI-Full-Color.png)

## Tabel of Content

1.  [Why](#introduction)
2.  [What](#what)
    1.  [Key Advantages of Travis-CI](#key-advantages)

<a name="introduction"></a>

## Introduction

Testing your work and not wildly adding new functionality is an important aspect of developing successful software. Otherwise, your software will end up in "integration hell" just like these design fails:

<img style='text-align: center' src="https://static.boredpanda.com/blog/wp-content/uploads/2021/01/home-interior-design-fails-22-5ff4266e4d4a8__700.jpg" alt="drawing" width="300"/>

Testing is of course important but costs a lot of time and therefore money. Also for testing, a lot of repetitive tasks have to be done. For example, the code must be compiled, bundled and the tests must be executed. If you are working in a team and you can't be sure that the other developers are working correctly. That's why you have to test their commits after every new pull. This is where Continuous Integration comes into play and Travis as a CI provider.

## Continuous Integration

Continuous integration (CI) describes the process of continuously merging components to form an application. The goal is to increase software quality, through early detection of errors. For this purpose, the software is built, automated tests are performed and software metrics are created to measure software quality (e.g. code coverage). This entire process is triggered automatically by checking into the version management system. After just a few minutes the developer receives feedback as to whether the quality requirements have been achieved or not.
Continuous Delivery (CD) is a further development of CI. Here the software is delivered immediately to the customer.

Advantages:

- Problems/Errors are quickly noticed
- Permanent availability of an working demo
- Shorter check-in intervals are generally advantageous, as this reduces the amount of merging effort required.

<img src="https://cloud.githubusercontent.com/assets/1128312/20186823/ddfdbb9e-a771-11e6-9e99-4720e7b60f53.png" alt="drawing" width="600"/>

## Travis CI

#### What is Travis CI?

Travis CI is a cloud hosted CI service that is itegrated with GitHub.

#### Features

Pro's:

- Open Source
- Easy setup & configuration
- Support for +21 languages
- Deployment on multiple cloud services (AWS, Heroku)
- Encrypted environment variables
- Parallel builds on multiple platforms
- Clean virtual machines after every build
- CLI client
- Fast and reliable
- Free to use for public GitHub projects

Con's:

- No support for own infrastructure
- No high security
- No build pipelines
- Debit Card for authentication required

# Getting Started

1. Log in on travis-ci.com with your GitHub Account
2. Authorize Travis CI

<img src="https://user-images.githubusercontent.com/7784660/42060974-e6384036-7b28-11e8-9aa1-1535dabe0dee.jpg" alt="drawing" width="600"/>

3. Activate GitHub Apps Integration

<img src="https://user-images.githubusercontent.com/7784660/42061901-feb97028-7b2b-11e8-9ac5-75e32a181087.jpg" alt="drawing" width="600"/>

4. Install Travis CI on all repositories or only on selected

<img src="https://user-images.githubusercontent.com/7784660/42060973-e61fe702-7b28-11e8-8e40-f99e26281750.jpg" alt="drawing" width="600"/>

5. Now you need to select a plan. In order to prevent misuse, Travis CI needs your debit card information even for the free plan

6. Create a .travis.yaml in the root folder of your project. More informations are provided below

7. Simply push your commit to start a build of your application via Travis CI

# .travis.yaml

The .travis.yaml (or .yml) is the heart piece of our CI setup.
Here we is mostly all of the configuration done.

We can select the language of the application:

```yaml
language: go
language: node_js
```

And the version:

```yaml
language: node_js
node_js:
  - "4"
```

Pick the os:

```yaml
os: osx

os: linux
dist: trusty
```

Declare custom commands:

```yaml
before_install:
  - sudo apt-get update -q
  - sudo apt-get install gcc-4.8 -y

script: make test
before_script: make pretest
after_script:  make clean

before_script:
  - npm customTest
```

Declare environment variables:

```yaml
env:
  - "user=master"
  - "password=supersecure"
```

Via the travis CLI-tool, you can setup encrypted variables:

```yaml
env:
  global:
    - secure: eslojc6DH5yMcfFjUiHpDYVLqxD+aDWsdNSi3CKavoZA44E...
```

You can use Docker: :whale:

```yaml
services:
  - docker

before_install:
  - docker pull ubuntu:20.04
```

Run multiple jobs:

```yaml
jobs:
  include:
    - name: "Lint"
      install: npm ci
      script: npm run lint

    - name: "Unit tests"
      install: npm ci
      script: npm run test:ci

    - name: "Build docker image"
      services:
        - docker
      install: skip
      script: docker build -t project:test .
```

Setup notifications:

```yaml
notifications:
  email:
    - sebastian@gmail.com
```

You can also integrate tools like Coveralls, DeepSource and 3rd Party Apps. In order to enhance your software quality.

# About this example

This is a very basic node.js project that uses [vows](http://vowsjs.org) as testing framework. In this project we just have a simple math function that is checked for correct output.

## Local Usage

Make sure you're in the root directory.

```sh
$ npm i
$ npm test
```

As you can see below, we did not need to define these commands in the .travis.yml, since Travis can run these by default.

```yaml
language: node_js
node_js:
  - node
  - "lts/*"
cache: npm
```

For more information have a look on [the documentation](http://about.travis-ci.org/) of Travis CI.

## Badges

Here is a status icon showing the state of this main branch:

[![Build Status](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio.svg?token=qf9Qoz1exW1V6BM8uvXA&branch=main)](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio)
