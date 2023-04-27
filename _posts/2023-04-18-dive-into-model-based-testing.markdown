---
layout: post
title: Dive into Model Based Testing
date: 2023-04-18 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: how-to-start.jpg # Add image post (optional)
tags: [quality-engineering] # add tag
---

### Why Model-Based Testing ?
I have heard of model-based testing but never applied it in a real project, not even in a proof-of-concept. The purpose of this post is to document what I find useful about Model-Based Testing and do a PoC using existing open-source projects. When I will have the chance to use it on a real project the content of this post and gained knowledge will allow me to come up faster with a solution.

*<u>Model-based testing</u> is a software testing technique where the run time behavior of the software under test is checked against predictions made by a model. A model is a description of a system’s behavior. Behavior can be described in terms of input sequences, actions, conditions, output, and flow of data from input to output.*

Software testing is an essential activity in software development and in a model-based process, test development and test execution can also be automated.

### When to use Model-Based Testing ?
Any software component that can be simulated by the model and compared (with expected results) is a candidate for model-based testing. The simplest model is an algorithm that takes inputs and creates a single output. 

Model-Based Testing is particularly good at finding memory leaks and potential conflicts that will cause the software to crash because automated tests can submit random input and run for extended periods of time.

Model-Based Testing can be used in following situations:
- *data flow* : the portion of testing focusses on the point from which variables receive the values till the point where these values are used
- *control flow* : testing structural changes of the software
- *dependency graphs* : the graph will represent the dependencies of several objects with each other
- *state transition* : the state of the software is tested against some inputs and notifies the system's behavior when the state transition took place
- *decision tables* : in this situation the actual results is compared to the expected results

Advantages of model-based testing:
- model-based tests can cover a large variety of scenarios with little effort
- design and specification errors can be spotted quickly
- improves test coverage
- early issue detection
- reduction in costs

The biggest advantage of using models to describe tests is the fact that they help in keeping the overview on what is actually tested. There goes the phrase "a picture is worth a thousand words". With model-based testing is not necessary to know all implementation details to start writing tests. In this type of testing is used a top-down modelling approach, the first version of the model has a high level of abstraction and does not yet contain any detailed test description.

### Proof of concept

### 1. GraphWalker


### 2. AltWalker


### Conclusion
The idea of model-based testing is pretty simple, these type of tests positively affects the efficiency and effectiveness of testing. Model-based testing helps in managing complexity and improves communication between quality engineers, developers, and product owners, bringing all parts involved on the same page. Also, those tests provide us better control over generated test cases, the diagrams help us in structuring and visualizing potential issues.

### Useful resources
[Model-based testing essentials book](https://www.amazon.com/Model-Based-Testing-Essentials-Certified-Foundation/dp/1119130018)

[List of MBT tools](http://mit.bme.hu/~micskeiz/pages/modelbased_testing.html)

[Simple tutorial](https://www.guru99.com/model-based-testing-tutorial.html)