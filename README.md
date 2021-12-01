[![Build Status](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio.svg?token=qf9Qoz1exW1V6BM8uvXA&branch=main)](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio)

# Travis CI ePortfolio

![Travis CI Logo](https://docs.travis-ci.com/images/TravisCI-Full-Color.png)

## Tabel of Content

1.  [Introduction](#introduction)
2.  [Continuous Integration](#continuous)
3.  [Travis CI](#Travis)
    1. [What is Travis CI?](#continuous)
    2. [Features](#Features)
    3. [Disadvantages](#Disadvantages)
4.  [Getting Started](#Getting)
5.  [.travis.yaml](#.travis)
6.  [About this example](#About)
    1. [Local Usage](#Local)
    2. [Badges](#Badges)
7.  [More](#More)

<a name="introduction"></a>

## Introduction

Testing your work and not wildly adding new functionality is an important aspect of successfully developing software. Otherwise, you and your software will end up in "integration hell". And look just like these design fails,
where the integration was not well thought out:

<img style='text-align: center' src="https://static.boredpanda.com/blog/wp-content/uploads/2021/01/home-interior-design-fails-22-5ff4266e4d4a8__700.jpg" alt="drawing" width="350"/>

Testing your software is essential but costs a lot of time and therefore money. A lot of repetitive tasks have to be done. For example, the code must be compiled, bundled and the tests must be executed. And if you are working in a team you can't rely on the other developers. Likely someone will eventually forget the testing before pushing their commit. That's why you would have to test after every single new pull. And this is where Travis CI as a Continuous Integration provider comes into play.

<a name="continuous"></a>

## Continuous Integration

Continuous integration (CI) describes the process of continuously merging small components and features to form an application. The goal is to increase software quality, through early detection of errors. For this purpose every single commit the software is built, automated tests are performed and software metrics are created to measure software quality (e.g. code coverage). This entire process is triggered automatically by checking into the version management system. After just a few minutes the developer receives feedback as to whether the quality requirements have been achieved or not.
Continuous Delivery (CD) is a further development of CI. Here the software will even be delivered immediately to the customer.

Advantages:

- Problems/Errors are quickly noticed
- Permanent availability of an working demo
- Shorter check-in intervals are generally advantageous, as this reduces the amount of merging effort required.

<img src="https://cloud.githubusercontent.com/assets/1128312/20186823/ddfdbb9e-a771-11e6-9e99-4720e7b60f53.png" alt="drawing" width="600"/>

<a name="Travis"></a>

## Travis CI

<a name="What"></a>

### 1. What is Travis CI?

Travis CI is a cloud hosted CI service that is useable with GitHub. It is easy to set up and free to use for public projects. About a million projects are already being verified with Travis CI and also in future Travics CI promises to remain free of charge.

In the picture below you can see a sample Travis CI worker. After a commit is pushed to GitHub, Travis CI triggers a new build on their servers. The necessary dependencies are loaded and the software is built. After this, Travis CI executes some predefined tests. If the build passes: Hooray!

Travis CI is also capable of deploying your code and notifying your team.

<img src="./example.png" alt="drawing" />

<a name="Features"></a>

### 2. Features

- Open Source
- Easy setup & configuration
- Support for +21 languages
- Deployment on multiple cloud services (AWS, Heroku...)
- Encrypted environment variables
- Parallel builds on multiple platforms
- Clean virtual machines after every build
- CLI client
- Fast and reliable
- Free to use for public GitHub projects

<a name="Disadvantages"></a>

### 3. Disadvantages:

- No support for own infrastructure
- No high security
- No build pipelines
- Debit Card for authentication required

<a name="Getting"></a>

# Getting Started

1. Sign in on travis-ci.com (You can use your GitHub-Account)
2. Authorize Travis CI

<img src="https://user-images.githubusercontent.com/7784660/42060974-e6384036-7b28-11e8-9aa1-1535dabe0dee.jpg" alt="drawing" width="600"/>

3. Activate GitHub Apps Integration

<img src="https://user-images.githubusercontent.com/7784660/42061901-feb97028-7b2b-11e8-9ac5-75e32a181087.jpg" alt="drawing" width="600"/>

4. Install Travis CI on all repositories or only on selected ones

<img src="https://user-images.githubusercontent.com/7784660/42060973-e61fe702-7b28-11e8-8e40-f99e26281750.jpg" alt="drawing" width="600"/>

5. Now you need to select a plan. In order to prevent misuse, Travis CI will need your debit card information even for the free plan

6. Create a .travis.yaml in the root folder of your project. More information is provided below

7. Now you simply push a commit to trigger a build and testing of your application via Travis CI

<a name=".travis"></a>

# .travis.yaml

The .travis.yaml (or .yml) is the heart piece of your CI setup.
This is where most of the configuration is done.

For example Travis CI needs to know the programming language of our application:

```yaml
language: go
```

And the version:

```yaml
language: node_js
node_js:
  - "4"
```

Pick the operating system of your machine:

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

Set environment variables:

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

In order to enhance your software quality, you can also integrate tools like [Coveralls](https://coveralls.io/), [DeepSource](https://deepsource.io/) and 3rd Party Apps.

<a name="About"></a>

# About this example

This is a very basic node.js project that uses [vows](http://vowsjs.org) as testing framework. In this project we just have a small function that is checked for correct output.

<a name="Local"></a>

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

<a name="Badges"></a>

## Badges

Here is a status icon showing the state of this main branch. This badge is provided by Travis CI and as you can see, the build is passing:

[![Build Status](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio.svg?token=qf9Qoz1exW1V6BM8uvXA&branch=main)](https://app.travis-ci.com/Sebastian-Zok/TravisCI-ePortfolio)

<a name="Badges"></a>

# More

For more information have a look on [the documentation](http://about.travis-ci.org/) of Travis CI.
