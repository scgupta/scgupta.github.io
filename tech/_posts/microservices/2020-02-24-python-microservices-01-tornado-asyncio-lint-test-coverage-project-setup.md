---
title: "Python Microservices: Choices, Key Concepts and Project Setup"
excerpt: "Tornado, with AsyncIO APIs, is an efficient framework for building IO-bound Python microservices. Learn key concepts. Set up lint, test, code coverage."
image: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png"
image_thumbnail: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s320/py-microservice-tornado-title-image.png"
tags:
  - "microservices"
  - "python"
  - "tornado"
canonical_url: "https://medium.com/slanglabs/python-microservices-01-tornado-asyncio-lint-test-coverage-project-setup-9fbf4ca3bf90"
code: true
---

*Distilled lessons from building microservices powering Slang Labs platform.*

![](https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png){: width="100%" class="framedimg"}

At SlangLabs, we are building a platform for *[programmers](https://slanglabs.in/developers.html){:target="_blank" rel="noopener nofollow"}* to easily and quickly add multilingual, multimodal *[Voice Augmented eXperiences (VAX)](https://medium.com/slanglabs/what-is-voice-augmented-experience-1003a28b6e5){:target="_blank" rel="noopener nofollow"}* to their mobile and web apps. Think of an assistant like Alexa or Siri, but *[running inside your app and tailored for your app](https://www.youtube.com/watch?v=TMhGGIAc0f0){:target="_blank" rel="noopener nofollow"}*.

The platform consists of:

- **Console** to configure a *[buddy](https://docs.slanglabs.in/slang/digging-deeper/building-buddies){:target="_blank" rel="noopener nofollow"}* for an app,
- **SDKs** for *[Android](https://docs.slanglabs.in/slang/getting-started/for-android-apps){:target="_blank" rel="noopener nofollow"}* and *[Web (JavaScript)](https://docs.slanglabs.in/slang/getting-started/for-web-apps){:target="_blank" rel="noopener nofollow"}* providing voice-to-action capabilities,
- **Microservices** that SDKs invoke to infer the *[intent](https://docs.slanglabs.in/slang/digging-deeper/building-buddies/intents){:target="_blank" rel="noopener nofollow"}* inherent in the voice utterance of an end-user, and extract associated *[entities](https://docs.slanglabs.in/slang/digging-deeper/building-buddies/entities-and-entity-types){:target="_blank" rel="noopener nofollow"}*, and
- **Analytics** to analyze [end-user behaviour](https://docs.slanglabs.in/slang/digging-deeper/understanding-analytics){:target="_blank" rel="noopener nofollow"} and improve the experience.

This series of blog posts is to share the best practices and lessons we have learned while building the microservices.

## Why Python

At the idea-phase of a startup, one has some sense of destination and direction but does not know exactly what to build. That clarity emerges only through iterations and experimentations. We were no different, so we had to pick a programming language and microservice framework suitable for rapid prototyping. These were our key considerations:

- **Rapid Development:** high velocity of experimentations for quick implementation and evaluation of ideas.
- **Performance:** lightweight yet mature microservice framework, efficient for mostly IO-bound application, scales to high throughput for concurrent requests.
- **Tools Infrastructure:** for automated testing, cloud deployment, monitoring.
- **Machine Learning (ML):** easy availability of libraries and frameworks.
- **Hiring:** access to talent and expertise.

There is no perfect choice for the programming language that ticks all of the above. It finally boils down to Python vs. Java/Scala because these are the only feasible languages for machine learning work. While Java has better performance and tooling, Python is apt for rapid prototyping. At that stage, we favoured rapid development and machine learning over other considerations, therefore picked Python.

Microservice architecture facilitates each service to independently choose the programming language and framework, and there is no need to standardize on one. However, a heterogeneous system adds DevOps and infra overheads, which we wanted to avoid as we were just a couple of guys hacking the system. With time, our team and platform grew and now has microservices in Go-lang and JavaScript too.

<figure class="aligncenter">
  <img src="https://3.bp.blogspot.com/-ENaoxjvdQnY/XlO9AdZGdWI/AAAAAAAATlA/DsdA5x4prWwGrDasG7QNO7oAZTAqg8qoACKgBGAsYHg/s1600/py-microservice-why-python.png" class="framedimg" >
  <figcaption class="aligncenter">Comparison of Programming Languages. 😀: Great, 😊: Nice, 😐: Ok, 😓: Bad.</figcaption>
</figure>


## Why Tornado

With Python, came its infamous Global Interpreter Lock. In brief, a thread can execute only if it has acquired the Python interpreter lock. Since it is a global lock, only one thread of the program can acquire it and therefore run at a time, even if the hardware has multiple CPUs. It effectively renders Python programs limited to single-threaded performance.

While GIL is a serious limitation for CPU-bound concurrent Python apps, for IO-bound apps, [cooperative multitasking](https://en.wikipedia.org/wiki/Cooperative_multitasking){:target="_blank" rel="noopener nofollow"} of [AsyncIO](https://docs.python.org/3/library/asyncio.html){:target="_blank" rel="noopener nofollow"} offers good performance (more about it later). For performance, we desired a web framework which is **lightweight** yet **mature**, and has **AsyncIO** APIs.

We evaluated three Python Web Frameworks: [Django](https://www.djangoproject.com/){:target="_blank" rel="noopener nofollow"}, [Flask](https://flask.palletsprojects.com/){:target="_blank" rel="noopener nofollow"}, and [Tornado](https://www.tornadoweb.org/){:target="_blank" rel="noopener nofollow"}.

- **Django** follows "batteries included" approach, it has everything you will need and more. While that eliminates integration compatibility blues, it also makes it bulky. It does not have AsyncIO APIs.
- **Flask**, on the other hand, is super lightweight and has a simple way of defining service endpoints through annotation. It does not have AsyncIO APIs.
- **Tornado** is somewhere between Django and Flask, it is neither as barebone as Flask nor as heavy as Django. It has quite a number of configurations, hooks, and nice testing framework. It had been having event-loop for scheduling cooperative tasks for much before AsyncIO, and had started supporting AsyncIO event loop and syntax.

> Tornado was just right for our needs. But most of our design tactics are independent of that choice, and are applicable regardless of the chosen web framework.

In recent time, more AsyncIO Python web frameworks are emerging: [Sanic](https://sanic.readthedocs.io/){:target="_blank" rel="noopener nofollow"}, [Vibora](https://vibora.io/){:target="_blank" rel="noopener nofollow"}, [Quart](https://pgjones.gitlab.io/quart/){:target="_blank" rel="noopener nofollow"}, [FastAPI](https://fastapi.tiangolo.com/){:target="_blank" rel="noopener nofollow"}. Even [Django is beginning to support async](https://docs.djangoproject.com/en/3.0/topics/async/){:target="_blank" rel="noopener nofollow"}.

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-67Hf2dkOCu8/XlPC_dbiF4I/AAAAAAAATlM/Ek4gCFvMOnIF-C0NiPzhc6xABLYlQGnxQCKgBGAsYHg/s1600/py-microservice-why-tornado.png" class="framedimg" >
  <figcaption class="aligncenter">Comparison of Python Web Frameworks. 😀: Great, 😊: Nice, 😐: Ok, 😓: Bad.</figcaption>
</figure>

## Overcoming Global Interpreter Lock

Before we plunge into design and code, let's understand some key concepts: cooperative multitasking, non-blocking calls, and AsyncIO.

### Preemptive vs Cooperative Multitasking

Threads follow the model of **preemptive multitasking**. Each thread executes one task. OS schedule a thread on a CPU, and after a fixed interval (or when the thread gets blocked typically due to an IO operation, whichever happens first), OS interrupts the thread and schedules another waiting thread on CPU. In this model of concurrency, multiple threads can execute parallelly on multiple CPUs, as well as interleaved on a single CPU.

In **cooperative multitasking**, there is a queue of tasks. When a task is scheduled for execution, it executes till a point of its choice (typically an IO wait) and yields control back to the event loop scheduler, which puts it the waiting queue, and schedules another task. At any time, only one task is executing, but it gives an appearance of concurrency.

<figure class="aligncenter">
  <img src="https://4.bp.blogspot.com/-XOcn-AXmQqs/XlPED6V1XTI/AAAAAAAATlY/gIZubD7Fllkb0xWwZih_yy2qvADvCImlgCKgBGAsYHg/s1600/py-microservice-cooperative-multitasking.png" class="framedimg" >
  <figcaption class="aligncenter">Preemptive Multitasking vs. Cooperative Multitasking.</figcaption>
</figure>

### Synchronous vs Asynchronous Calls

In **synchronous** or **blocking** function calls, the control returns back to the caller only after completion. Consider the following pseudocode:

~~~ python
bytes = read()
print(bytes)
print("done")

# "done" is printed only *after* bytes.
~~~

In **asynchronous** or **non-blocking** function calls, the control returns immediately to the caller. The called function can pause while execution. It takes a callback routine as an argument, and when called function finishes and results are ready, it invokes the callback with results. Meanwhile, the caller function resumes execution even before completion of the called function. Assume there is a non-blocking async_read function, which takes a callback function, and calls it with the read bytes. Consider the following pseudocode:

~~~ python
asyn_read(print)
print("done")

# "done" may be printed *before* bytes.
~~~

As you can see asynchronous code with callbacks is hard to understand because the execution order of the code can be different from the lexical order.

### Python AsyncIO

AsyncIO syntax of async and await facilitates writing asynchronous code in synchronous style instead of using callbacks, making code easy to understand.

~~~ python
import asyncio

async def f():
    bytes = await async_read()
    # f pauses here, yields control.
    # Resumes when result (bytes) is ready.
    print(bytes)
    print("done")

asyncio.run(f())  # Append f() to the IO Event Loop queue
~~~

When a function is **async**, it is called **coroutine**. It must be awaited, as its results will be available only in future. An **await** expression **yields** the control to the scheduler. Code after the await expression is like a callback, the control to be resumed here later when coroutine completes and results are ready.

AsyncIO has an IO Event Loop, a queue that holds all completed coroutines ready to be resumed.


## Derisking by Design

While Tornado has worked out well for us so far, we did not know it then. We designed our microservices such that Tornado-dependent code was segregated and localized. It was to easily migrate to a different framework if the need arises. Regardless, it is a good idea to structure your microservice into two layers: Web Framework Layer and framework independent Service Layer.

### Web Framework Layer

Web Framework Layer is responsible for REST service endpoints over HTTP protocols. It does not have any business logic. It processes incoming requests, extract relevant information from the payload, and calls a function in the Service Layer which performs business logic. It packages the returned results appropriately and sends the response. For Tornado, it consists of two files:

- **server.py** contains an HTTP server that starts the event loop and application.
- **app.py** contains endpoint routes that map REST API to a function in the service layer (specifically to a function in service.py, see next).

### Service Layer

Service Layer contains only business logic, and knows nothing about HTTP or REST. That allows any communication protocol to be stitched on top of it without touching business logic. There is only one requirement for this layer:

- **service.py** must contain all functions needed to implement the service endpoints. Think of it as logical service APIs, independent of any Web framework or communication protocol.

Logical service APIs allow the Web Framework Layer to be implemented (and replaced) without getting into the nitty-gritty of the inner working of the service. It also facilitates standardization and sharing of a large portion of web framework code across services.


## Testing

We are rare among startups to automate testing and code coverage from the very beginning. It may appear counter-intuitive but we did it to maintain high velocity, and fearlessly change any part of the system. Tests offered us a safety net needed while developing in a dynamically-typed interpreted language. It was also partly due to paranoia regarding our non-obvious choice of Tornado, to safeguard us in case we need to change it.

There are three types of tests:

- **Unit Tests:** Limited to independently test a class or function, mostly for leaf-level independent classes/functions.
- **Integration Tests:** To test the working of multiple classes and functions. Out of process or network API calls (such as databases and other services) are mocked.
- **End-to-End Tests:** To test deployment on test or stage environment. Nothing is mocked, just that data is not from the prod environment and may be synthetic.

We wrote integration tests both for Service Layer to test business logic, as well for Web Framework Layer to test the functioning of REST endpoints in Tornado server.

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-6T77KXkI7p8/XlPGRDOGDsI/AAAAAAAATlk/6sGk8Zxe8pUnPPMBk_yFuX0rYhPWrZlCwCKgBGAsYHg/s1600/py-microservice-framework-seperation-testing.png" class="framedimg" >
  <figcaption class="aligncenter">Tests for Tornado and Service Layers.</figcaption>
</figure>

## Project Setup

### Get Source Code

Clone the [GitHub repo](https://github.com/scgupta/tutorial-python-microservice-tornado) and inspect the content:

~~~ bash
$ git clone https://github.com/scgupta/tutorial-python-microservice-tornado.git

$ cd tutorial-python-microservice-tornado
$ git checkout -b <branch> tag-01-project-setup

$ tree .
.
├── LICENSE
├── README.md
├── addrservice
│   └── __init__.py
├── requirements.txt
├── run.py
└── tests
    ├── __init__.py
    ├── integration
    │   └── __init__.py
    └── unit
        └── __init__.py
~~~

The directory `addrservice` is  for the source code of the service, and the directory `test` is for keeping the tests.

### Setup Virtual Environment

Using a virtual environment is one of the best practices, especially when you work on multiple projects. Create one for this project, and install the dependencies from `requirements.txt` :

~~~ bash
$ python3 -m venv .venv
$ source ./.venv/bin/activate
$ pip install --upgrade pip
$ pip3 install -r ./requirements.txt
~~~

### Tools

The script `run.py` is a handy utility script to run static type checker, linter, unit tests, and code coverage. In this series, you will see that using these tools from the very beginning is actually most economical, and does not add perceived overhead.
Let's try running these. In each of the following, you can use either of the commands.

**Static Type Checker:** [mypy](http://mypy-lang.org/){:target="_blank" rel="noopener nofollow"} package

~~~ bash
$ mypy ./addrservice ./tests

$ ./run.py typecheck
~~~

**Linter:** [flake8](https://flake8.pycqa.org/){:target="_blank" rel="noopener nofollow"} package

~~~ bash
$ flake8 ./addrservice ./tests

$ ./run.py lint
~~~

**Unit Tests:** Python [unittest](https://docs.python.org/3/library/unittest.html){:target="_blank" rel="noopener nofollow"} framework

~~~ bash
$ python -m unittest discover tests -p '*_test.py'

$ ./run.py test
~~~

This will run all tests in the directory `tests`. You can run unit or integration test suites (in `tests/unit` and `tests/integration` directories respectively) as following:

~~~ bash
$ ./run.py test --suite unit
$ ./run.py test --suite integration
~~~

**Code Coverage:** [coverage](https://coverage.readthedocs.io/){:target="_blank" rel="noopener nofollow"} package

~~~ bash
$ coverage run --source=addrservice --branch -m unittest discover tests -p '*_test.py'

$ coverage run --source=addrservice --branch ./run.py test
~~~

After running tests with code coverage, you can get the report:

~~~ bash
$ coverage report
Name                      Stmts   Miss Branch BrPart  Cover
-----------------------------------------------------------
addrservice/__init__.py       2      2      0      0     0%
~~~

You can also generate HTML report:

~~~
$ coverage html
$ open htmlcov/index.html
~~~

If you are able to run all these commands, your project setup is complete.

## Summary

There are several choices for building machine learning: Java, JavaScript, Python, and Go. If microservice involves interfacing with ML libs, choices reduce to  Java and Python. 

For quick prototyping, Python is more suitable. But it comes with the drawback of Global Interpreter Lock. Cooperative multitasking with non-blocking asynchronous calls using asyncio comes to rescue. Tornado is the only mature Python web framework with asyncio APIs. 

Layered design can derisk in case framework is to be changed in future. Tests can also be layered: unit, integration, and end-to-end. It is easy to setup lint, test, code coverage from the very beginning of the project.
