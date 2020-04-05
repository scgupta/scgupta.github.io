---
title: "Python Microservices: Effective Canonical Logging across Services"
excerpt: "Learn how to design, implement, test and configure canonical logging across microservices using Python and Tornado web framework."
image: "https://2.bp.blogspot.com/-d7uBujsQa7k/XnDo8nhxKqI/AAAAAAAAT2E/l9oVZPtBaUUkg2j8uijPnN0sirqx4R5eQCKgBGAsYHg/s1600/shell-1361911.jpg"
image_thumbnail: "https://2.bp.blogspot.com/-d7uBujsQa7k/XnDo8nhxKqI/AAAAAAAAT2E/l9oVZPtBaUUkg2j8uijPnN0sirqx4R5eQCKgBGAsYHg/s320/shell-1361911.jpg"
tags:
  - "microservices"
  - "python"
  - "tornado"
canonical_url: https://www.slanglabs.in/blog/python-microservices-03-effective-canonical-logging-across-services
code: true
---

*Distilled lessons from building microservices powering Slang Labs platform.*

<figure class="aligncenter">
  <img src="https://2.bp.blogspot.com/-d7uBujsQa7k/XnDo8nhxKqI/AAAAAAAAT2E/l9oVZPtBaUUkg2j8uijPnN0sirqx4R5eQCKgBGAsYHg/s1600/shell-1361911.jpg" alt="Seashell and annual rings in a tree log" width="100%" class="framedimg" >
  <figcaption class="aligncenter">Image by <a href="https://pixabay.com/users/fitschen-3851757/" target="_blank" rel="nofollow">Friedrich Frühling</a> from <a href="https://pixabay.com/photos/shell-tree-rings-annual-rings-1361911/" target="_blank" rel="nofollow">Pixabay</a></figcaption>
</figure>

Nature is a meticulous logger, and its logs are beautiful. Calcium carbonate layers in a seashell are nature’s log of ocean temperature, water quality, and food supply. Annual rings in tree cambium are nature’s log of dry and rainy seasons and forest fires. Fossils in the layers in sedimentary rocks are nature’s log of the flora and fauna life that existed at the time.

In software projects, logs, like tests, are often afterthoughts. But at [Slang Labs](https://slanglabs.in){:target="_blank" rel="nofollow"}, we take inspiration from nature's elegance and meticulousness.

We are building a platform for *[programmers](https://slanglabs.in/developers.html){:target="_blank" rel="nofollow"}* to make interaction with their mobile and web apps more natural by adding *[Voice Augmented eXperiences (VAX)](https://medium.com/slanglabs/what-is-voice-augmented-experience-1003a28b6e5){:target="_blank" rel="nofollow"}*. The platform is powered by a collection of microservices. Each log entry a microservice emits is a fossil record of a request. Logs are designed for timely and effective use in raising alerts and swift diagnosis of issues.

This blog post captures the essence of our logging tactics:

- **Requirements:** what we needed from logging, and why.
- **Implementation:** how we implemented these requirements in Python and Tornado.
- **Testing:** how we examine logs manually, and if needed in the unit and integration tests.

---

## Requirements

### We want to use off-the-shelf log tools.

Programmers love to write code and build tools. But we don’t suffer Not Invented Here syndrome. No glory in reinventing the wheel.

We wanted to use existing log processing tools. These tools work on various log formats: [Syslog](https://tools.ietf.org/html/rfc5424){:target="_blank" rel="nofollow"}, [Common Log Format](https://httpd.apache.org/docs/current/logs.html#common){:target="_blank" rel="nofollow"}, [W3 Extended Log File Format](https://www.w3.org/TR/WD-logfile.html){:target="_blank" rel="nofollow"}, and JSON log formats.

We needed a structured logging format that is reasonably human-readable for manual examination. We found [Logfmt](https://github.com/kr/logfmt){:target="_blank" rel="nofollow"}, that encodes as a string of *key-value* pairs, offers a good balance.

### We want to diagnose an issue quickly.

While debugging issues, we realized that combing through logs is time-consuming. It is because logs for a request are scattered and interleaved with logs for other requests.

We liked the idea of [canonical log lines](https://brandur.org/canonical-log-lines){:target="_blank" rel="nofollow"}: *consolidating* all log info in a **single log entry for each request**. For each incoming request, our microservices emit a single logline at the time of sending the response.

### We want to debug across microservices.

We need to diagnose the **complete life** of a request as it passes through many microservices.

We assign a request id at the beginning and pass it along to the other services. It is sent as an attribute in the HTTP header, or a query parameter, or in the body payload.

The request-id is used to string together all calls made to internal microservices.

### We want to multicast logs.

Logs are useful for debugging. But we also use it for analytics, dashboards, monitoring, and alerts.

We desired the ability to integrate with multiple tools. We prefer using logger **configuration** to channel log entries to multiple stores. Glue code, if any, must be localized.

---

## Implementation

Logging implementation has the following pieces:

- **Log Context:** utility functions to maintain context (key-value map) during the life of a request.
- **Log Format:** utility function that examines the accumulated context and emits a canonical log entry in logfmt format.
- **Tornado Request Handlers:** wiring various Tornado hooks to use these utility functions.
- **Tornado Server:** replacing the default task factory with the one that maintains and switches context while processing concurrent requests.
- **Log Configuration:** setting log multicast to various destinations.


Enough of talk.

> Talk is cheap. Show me the code.
> - Linus Torvalds

### Get the source code:

~~~ bash
$ git clone https://github.com/scgupta/tutorial-python-microservice-tornado.git

$ cd tutorial-python-microservice-tornado
$ git checkout -b <branch> tag-03-logging

$ python3 -m venv .venv
$ source ./.venv/bin/activate
$ pip install --upgrade pip
$ pip3 install -r ./requirements.txt

$ tree .
.
├── LICENSE
├── README.md
├── addrservice
│   ├── service.py
│   ├── tornado
│   │   ├── app.py
│   │   └── server.py
│   └── utils
│       └── logutils.py
├── configs
│   └── addressbook-local.yaml
├── data
│   └── addresses
│       ├── namo.json
│       └── raga.json
├── requirements.txt
├── run.py
└── tests
    ├── integration
    │   └── tornado_app_addreservice_handlers_test.py
    └── unit
        └── tornado_app_handlers_test.py
$ ./run.py test
~~~

### Log Context utility functions:

A context is maintained throughout the life of a request. At any stage, more key-value pairs can be added to the context. All these pairs are logged in a single entry at the time of sending the response.

We use [aiotask-context](https://github.com/Skyscanner/aiotask-context){:target="_blank" rel="nofollow"} package for that. Other alternatives are [aiocontext](https://aiocontext.readthedocs.io/en/latest/introduction.html){:target="_blank" rel="nofollow"}, and [tasklocals](https://github.com/vkryachko/tasklocals){:target="_blank" rel="nofollow"}. There is also [contextvars](https://docs.python.org/3/library/contextvars.html){:target="_blank" rel="nofollow"} standard Python package if you are using Python 3.7+.

The complexity of the context management is encapsulated in easy to use utility functions to get and set key-value pairs in the context.

~~~ python
# addrservice/utils/logutils.py

import aiotask_context as context

LOG_CONTEXT = 'log_context'

def get_log_context() -> Dict:
    log_context = context.get(LOG_CONTEXT)
    if log_context is None:
        log_context = {}
        context.set(LOG_CONTEXT, log_context)
    return log_context

def set_log_context(**kwargs) -> None:
    log_context = get_log_context()
    log_context.update(kwargs)

def clear_log_context() -> None:
    log_context = get_log_context()
    log_context.clear()
~~~

### Log Formatting utility function:

Python [logfmt](https://pypi.org/project/logfmt/0.1/){:target="_blank" rel="nofollow"} package does the heavy lifting of emitting the log. It takes a dictionary, and convert it to a string consisting of *key=value*"* pairs.

A log function encapsulates all this processing. It adheres to [Python logger API](https://docs.python.org/3/library/logging.html#logging.Logger.debug){:target="_blank" rel="nofollow"}. In case of an exception, it stores `exc_info` traceback in logs.

~~~ python
# addrservice/utils/logutils.py

import logfmt
import logging
import re
import traceback

def log(
    logger: logging.Logger,
    lvl: int,
    include_context: bool = False,
    **kwargs
) -> None:
    all_info = {**get_log_context(), **kwargs} \
                if include_context else kwargs
    info = {
        k: v for k, v in all_info.items()
        if k not in ['exc_info', 'stack_info', 'extra']
    }
    exc_info = all_info.get('exc_info')
    if exc_info:  # tuple (typ, value, tb)
        trace = '\t'.join(traceback.format_exception(*exc_info))
        info['trace'] = re.sub(r'[\r\n]+', '\t', trace)
    msg = next(logfmt.format(info))
    logger.log(
        lvl, msg,
    )
~~~

### Tornado Request Handlers:

It is time to use the context and log format utility functions.

We use [Tornado to develop microservices](/tech/python-microservices-02-tornado-rest-unit-integration-tests.html){:target="_blank"}. It provides various hooks to shape logging and error reporting behavior.

It has a [`log_function`](https://www.tornadoweb.org/en/stable/web.html#tornado.web.Application.settings){:target="_blank" rel="nofollow"} setting to pass a function, that Tornado calls at the end of every request to log the result. We implement this method to emit a log entry for the request. It includes all *key-value* pairs in the context.

~~~ python
# addrservice/tornado/app.py

def log_function(handler: tornado.web.RequestHandler) -> None:
    logger = getattr(handler, 'logger',
                logging.getLogger(LOGGER_NAME))
    if handler.get_status() < 400:
        level = logging.INFO
    elif handler.get_status() < 500:
        level = logging.WARNING
    else:
        level = logging.ERROR
    logutils.log(
        logger,
        level,
        include_context=True,
        message='RESPONSE',
        status=handler.get_status(),
        time_ms=(1000.0 * handler.request.request_time())
    )
    logutils.clear_log_context()
~~~

Other hooks are:

- [`write_error`](https://www.tornadoweb.org/en/stable/web.html#tornado.web.RequestHandler.write_error){:target="_blank" rel="nofollow"} to customize the error response. Information about the error is added to the log context.
- [`log_exception`](https://www.tornadoweb.org/en/stable/web.html#tornado.web.RequestHandler.log_exception){:target="_blank" rel="nofollow"} to log uncaught exceptions. It can be overwritten to log in logfmt format.

These functions are overridden in the base request handler. A `req_id` is assigned to the incoming request, and stored in the log context.

~~~ python
# addrservice/tornado/app.py

class BaseRequestHandler(tornado.web.RequestHandler):
    def initialize(
        self,
        service: AddressBookService,
        config: Dict,
        logger: logging.Logger
    ) -> None:
        self.service = service
        self.config = config
        self.logger = logger

    def prepare(self) -> Optional[Awaitable[None]]:
        req_id = uuid.uuid4().hex
        logutils.set_log_context(
            req_id=req_id,
            method=self.request.method,
            uri=self.request.uri,
            ip=self.request.remote_ip
        )

        return super().prepare()

    def write_error(self, status_code: int, **kwargs: Any) -> None:
        self.set_header(
            'Content-Type', 'application/json; charset=UTF-8')
        body = {
            'method': self.request.method,
            'uri': self.request.path,
            'code': status_code,
            'message': self._reason
        }

        logutils.set_log_context(reason=self._reason)

        if 'exc_info' in kwargs:
            exc_info = kwargs['exc_info']
            logutils.set_log_context(exc_info=exc_info)
            if self.settings.get('serve_traceback'):
                # in debug mode, send a traceback
                trace = '\n'.join(
                    traceback.format_exception(*exc_info))
                body['trace'] = trace

        self.finish(body)

    def log_exception(
        self,
        typ: Optional[Type[BaseException]],
        value: Optional[BaseException],
        tb: Optional[TracebackType],
    ) -> None:
        if isinstance(value, tornado.web.HTTPError):
            if value.log_message:
                msg = value.log_message % value.args
                logutils.log(
                    tornado.log.gen_log,
                    logging.WARNING,
                    status=value.status_code,
                    request_summary=self._request_summary(),
                    message=msg
                )
        else:
            logutils.log(
                tornado.log.app_log,
                logging.ERROR,
                message='Uncaught exception',
                request_summary=self._request_summary(),
                request=repr(self.request),
                exc_info=(typ, value, tb)
            )
~~~

This completes the wiring in Tornado request handlers for logging canonical log entry in logfmt format for each request for all successful responses, errors, and uncaught exceptions.

At all locations, information is stuffed in the key-value form in the task context. At the time of sending the response, Tornado calls `log_function`, which unpacks the context, and logs it in logfmt.

### Tornado Server:

Tornado and asyncio work on the principle of [cooperative multitasking](/tech/python-microservices-01-tornado-asyncio-lint-test-coverage-project-setup.html){:target="_blank"}. Multiple requests (with associated context) can exist at a given time. So the context has to be preserved across interleaving of these requests.

The `aiotask_context` requires a different task factory to be set for the `asyncio` event loop. It must be done while starting the HTTP server.

~~~ python
# addrservice/tornado/server.py

def run_server(
    app: tornado.web.Application,
    service: AddressBookService,
    config: Dict,
    port: int,
    debug: bool,
    logger: logging.Logger
):
    name = config['service']['name']
    loop = asyncio.get_event_loop()
    loop.set_task_factory(context.task_factory)

    # ... Rest of the method
~~~

### Log Configuration for multicasting:

Python's standard [logging](https://docs.python.org/3/library/logging.html){:target="_blank" rel="nofollow"} package documentation has an excellent [HOWTO](https://docs.python.org/3/howto/logging.html){:target="_blank" rel="nofollow"} guide and [Cookbook](https://docs.python.org/3/howto/logging-cookbook.html){:target="_blank" rel="nofollow"}. These are rich sources of information, and doesn't leave much to add. Following are some of the best practices:

- Do NOT use ROOT logger directly through `logging.debug()`, `logging.error()` methods directly because it is easy to overlook their default behavior.
- Do NOT use module loggers of variety `logging.getLogger(__name__)` because any complex project will require controlling logging through configuration (see next point). These may cause surprise if you forget to set `disable_existing_loggers` to false or overlook how modules are loaded and initialized. If use at all, call `logging.getLogger(__name__)` inside function, rather than outside at the beginning of a module.
- `dictConfig` (in `yaml`) offers right balance of versatility and flexibility compared to `ini`-based `fileConfig` or doing it in code. Specifying logger in config files allows you to use different logging levels and infra in prod deployment, stage deployments, and local debugging (with increasingly more logs).

Each log entry has to be relayed to multiple data-store destinations. This can be achieved by setting the log configuration for the Python logging package. It has to be a yaml file using the format defined by the logging.config package.

Log configuration is in a yaml file. It uses the format required by the [`logging.config`](https://docs.python.org/3/library/logging.config.html){:target="_blank" rel="nofollow"} package.

The config defines two log formats:

- **brief** to display on the console; log prefix is kept simple.
- **detailed** to log into a file; it has all fields in *key-value* pairs.

~~~ yaml
# configs/addressbook-local.yaml

service:
  name: Address Book

logging:
  version: 1
  formatters:
    brief:
      format: '%(asctime)s %(name)s %(levelname)s : %(message)s'
    detailed:
      format: 'time="%(asctime)s" logger="%(name)s" level="%(levelname)s" file="%(filename)s" lineno=%(lineno)d function="%(funcName)s" %(message)s'
  handlers:
    console:
      class: logging.StreamHandler
      level: INFO
      formatter: brief
      stream: ext://sys.stdout
    file:
      class : logging.handlers.RotatingFileHandler
      level: DEBUG
      formatter: detailed
      filename: /tmp/addrservice-app.log
      backupCount: 3
  loggers:
    addrservice:
      level: DEBUG
      handlers:
        - console
        - file
      propagate: no
    tornado.access:
      level: DEBUG
      handlers:
        - file
    tornado.application:
      level: DEBUG
      handlers:
        - file
    tornado.general:
      level: DEBUG
      handlers:
        - file
  root:
    level: WARNING
    handlers:
      - console
~~~

Notice that this configuration not just defines a logger `addrservice` for this service, but also modifies behavior of Tornado's general logger. There are several pre-defined [handlers](https://docs.python.org/3/library/logging.handlers.html){:target="_blank" rel="nofollow"}. Here the `SteamHandler` and `RotatingFileHandler` are being used to write to console and log files respectively.

The first thing the server does is reading this config and setting up the loggers.

~~~ python
# addrservice/tornado/server.py

def main(args=parse_args()):
    config = yaml.load(args.config.read(), Loader=yaml.SafeLoader)

    logging.config.dictConfig(config['logging'])
    logger = logging.getLogger(LOGGER_NAME)

    addr_service, addr_app = make_addrservice_app(
        config, args.debug, logger)

    run_server(
        app=addr_app,
        service=addr_service,
        config=config,
        port=args.port,
        debug=args.debug,
        logger=logger
    )
~~~

---

## Manual Testing

In the previous section, we wired the logging to use logfmt format, maintain a context for each request, emit log only at the end, and multicast the logs to many destinations.

Logs are processed in real-time. Entries are grouped based on the presence and value of keys (from key-value pairs). We have a dashboard showing various kinds of errors in all services. When any threshold is breached, we get automated alters.

That automation deserves a separate blog post in its own right. Here we will examine only the log output. It looks like a row in a NoSQL data store. Let’s see it in action.

### Start the server

It will show the console log:

~~~ bash
$ python3 addrservice/tornado/server.py --port 8080 --config ./configs/addressbook-local.yaml --debug

2020-03-17 12:54:15,198 addrservice INFO : message="STARTING" service_name="Address Book" port=8080
~~~

### Watch the log

~~~ bash
$ tail -f /tmp/addrservice-app.log

time="2020-03-17 12:54:15,198" logger="addrservice" level="INFO" file="logutils.py" lineno=57 function="log" message="STARTING" service_name="Address Book" port=8080
~~~

### Send a reqest

~~~ bash
$ curl -i -X POST http://localhost:8080/addresses -d '{"name": "Bill Gates"}'

HTTP/1.1 201 Created
Server: TornadoServer/6.0.3
Content-Type: text/html; charset=UTF-8
Date: Tue, 17 Mar 2020 07:26:32 GMT
Location: /addresses/7feec2df29fd4b039028ad351bafe422
Content-Length: 0
Vary: Accept-Encoding
~~~

The console log will show brief log entries:

~~~ bash
2020-03-17 12:56:32,784 addrservice INFO : req_id="e6cd3072530f46b9932946fd65a13779" method="POST" uri="/addresses" ip="::1" message="RESPONSE" status=201 time_ms=1.2888908386230469
~~~

The log file will show logfmt-style one-line detailed canonical log entries:

~~~ bash
time="2020-03-17 12:56:32,784" logger="addrservice" level="INFO" file="logutils.py" lineno=57 function="log" req_id="e6cd3072530f46b9932946fd65a13779" method="POST" uri="/addresses" ip="::1" message="RESPONSE" status=201 time_ms=1.2888908386230469
~~~

---

## Unit and Integration Tests

Tests are configured to be quiet:

~~~ bash
$ coverage run --source=addrservice \
    --omit="addrservice/tornado/server.py" \
    --branch ./run.py test

..
----------------------------------------------------------------------
Ran 2 tests in 0.049s

OK

$ coverage report

Name                              Stmts   Miss Branch BrPart  Cover
-------------------------------------------------------------------
addrservice/__init__.py               4      0      0      0   100%
addrservice/service.py               25      1      0      0    96%
addrservice/tornado/__init__.py       0      0      0      0   100%
addrservice/tornado/app.py          105      6     18      6    90%
addrservice/utils/__init__.py         0      0      0      0   100%
addrservice/utils/logutils.py        28      0      6      0   100%
-------------------------------------------------------------------
TOTAL                               162      7     24      6    93%
~~~

If you want to change the log message during tests, change log level from `ERROR` to `INFO`:

~~~ python
# tests/unit/tornado_app_handlers_test.py
IN_MEMORY_CFG_TXT = '''
service:
  name: Address Book Test
logging:
  version: 1
  root:
    level: INFO
'''
~~~

---

With that change, if you run the tests, you can examine the logs:

~~~ bash
$ ./run.py tests

INFO:addrservice:req_id="a100e35140604d72930cb16c9eed8e8a" method="GET" uri="/addresses/" ip="127.0.0.1" message="RESPONSE" status=200 time_ms=1.232147216796875
INFO:addrservice:req_id="29b08c81acbd403b89f007ba03b5fee7" method="POST" uri="/addresses/" ip="127.0.0.1" message="RESPONSE" status=201 time_ms=0.9398460388183594
WARNING:addrservice:req_id="1c959a77f9de4f7e87e384a174fb6fbe" method="POST" uri="/addresses/" ip="127.0.0.1" reason="Invalid JSON body" message="RESPONSE" status=400 time_ms=1.7652511596679688 trace="Traceback.....
~~~

---

## Summary

For effective logging in microservices, you should:

- **Log Format:** Pick a standard format suitable for off-the-shelf log processing tools and humans.
- **Canonical Entry:** Emit one log entry for each request at the response time consolidating all log info. It makes debugging easy.
- **Request Id:** Assign an id to each request to identify all calls made to internal services for serving a request.
- **Configuration:** Use logger configuration to channel log entries to multiple stores.
