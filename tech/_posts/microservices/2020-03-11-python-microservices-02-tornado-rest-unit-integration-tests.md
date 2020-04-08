---
title: "Python Microservices: Build and Test REST endpoints with Tornado"
excerpt: "Learn to implement Tornado HTTP endpoints as a layer on business logic. Tune it to assist debugging, and write unit and integration tests."
image: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png"
image_thumbnail: "https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s320/py-microservice-tornado-title-image.png"
tags:
  - "microservices"
  - "python"
  - "tornado"
canonical_url: "https://www.slanglabs.in/blog/python-microservices-02-tornado-rest-unit-integration-tests"
code: true
---

*Distilled lessons from building microservices powering Slang Labs platform.*

![](https://3.bp.blogspot.com/-XfGIp29o7pA/XlNCLext4FI/AAAAAAAATkM/doyTIgviBXIe_hP9MD-x4-xLS07nqkbtQCKgBGAsYHg/s1600/py-microservice-tornado-title-image.png){: width="100%" class="framedimg"}

At SlangLabs, we are building a platform for *[programmers](https://slanglabs.in/developers.html){:target="_blank" rel="nofollow"}* to easily and quickly add multilingual, multimodal *[Voice Augmented eXperiences (VAX)](https://medium.com/slanglabs/what-is-voice-augmented-experience-1003a28b6e5){:target="_blank" rel="nofollow"}* to their mobile and web apps. Think of an assistant like Alexa or Siri, but *[running inside your app and tailored for your app](https://www.youtube.com/watch?v=TMhGGIAc0f0){:target="_blank" rel="nofollow"}*.

The platform is powered by a collection of microservices. For implementing these services, we chose [Tornado because it has AsyncIO APIs](/tech/python-microservices-01-tornado-asyncio-lint-test-coverage-project-setup.html). It is not heavyweight. Yet, it is mature and has a number of configurations, hooks, and a nice testing framework.

This blog post covers some of the best practices we learned while building these services; how to:

- Design REST endpoints as a separate layer over business logic,
- Implement Tornado HTTP server and service endpoint handlers,
- Use Tornado hooks to control behavior and assist debugging, and
- Write unit and integration tests using Tornado testing infra.

---

## Application REST Endpoints

As an example, we will but a [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){:target="_blank" rel="nofollow"} microservice for an address-book using Tornado:

- **Create an address:** `POST /addresses`  
Returns HTTP status 201 upon adding successfully, and 400 if request body payload is malformed. The request body should have the new address entry in JSON format. The id the newly created address is sent back in the Location attribute of the header of the HTTP response.
- **Read an address:** `GET /addresses/{id}`  
Returns 404 if the id doesn't exist, else returns 200. The response body contains the address in JSON format.
- **Update an address:** `PUT /addresses/{id}`  
Returns 204 upon updating successfully, 404 if the request body is malformed, and 404 if the id doesn't exist. The request body should have the new value of the address.
- **Delete an address:** `DELETE /addresses/{id}`  
Returns 204 upon deleting the address, 404 is id doesn't exist.
- **List all addresses:** `GET /addresses`  
Returns 200, and the response body with all addresses in the address book.

In case of an error (i.e. when return status code is 4xx or 5xx), the response body has JSON describing the error.

You may want to refer to the list of [HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status){:target="_blank" rel="nofollow"}, and [best practices for REST API design](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/){:target="_blank" rel="nofollow"}.

By the end of this blog post, you will know how to implement and test these endpoints.

---

## Get Source Code

Clone the [GitHub repo](https://github.com/scgupta/tutorial-python-microservice-tornado) and inspect the content:

~~~ bash
$ git clone https://github.com/scgupta/tutorial-python-microservice-tornado.git

$ cd tutorial-python-microservice-tornado
$ git checkout -b <branch> tag-02-microservice

$ tree .
.
├── LICENSE
├── README.md
├── addrservice
│   ├── service.py
│   └── tornado
│       ├── app.py
│       └── server.py
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
~~~

The service endpoints and tests are implemented in the highlighted files in the listing above.

Setup a virtual environment, and install the dependencies from `requirements.txt`. Run tests as a sanity check.

~~~ bash
$ python3 -m venv .venv
$ source ./.venv/bin/activate
$ pip install --upgrade pip
$ pip3 install -r ./requirements.txt

$ ./run.py test
~~~

---

## Layered Design

The address service will be implemented in two layers:

- **Service Layer** contains all the business logic and knows nothing about REST and HTTP.
- **Web Framework Layer** contains REST service endpoints over HTTP protocol and knows nothing about business logic.

<figure class="aligncenter">
  <img src="https://1.bp.blogspot.com/-6T77KXkI7p8/XlPGRDOGDsI/AAAAAAAATlk/6sGk8Zxe8pUnPPMBk_yFuX0rYhPWrZlCwCKgBGAsYHg/s1600/py-microservice-framework-seperation-testing.png" class="framedimg" >
  <figcaption class="aligncenter">Service and Web Framework (Tornado) layers.</figcaption>
</figure>

The Service Layer exposes the function APIs for various CRUD operations to be used by the Web Framework layer.

Since the focus of this article is on the Web Framework layer, the Service layer is implemented as simple stubs.

~~~ python
# addrservice/service.py

from typing import Dict
import uuid

class AddressBookService:
    def __init__(self, config: Dict) -> None:
        self.addrs: Dict[str, Dict] = {}

    def start(self):
        self.addrs = {}

    def stop(self):
        pass

    async def create_address(self, value: Dict) -> str:
        key = uuid.uuid4().hex
        self.addrs[key] = value
        return key

    async def get_address(self, key: str) -> Dict:
        return self.addrs[key]

    async def update_address(self, key: str, value: Dict) -> None:
        self.addrs[key]  # will cause exception if key doesn't exist
        self.addrs[key] = value

    async def delete_address(self, key: str) -> None:
        self.addrs[key]  # will cause exception if key doesn't exist
        del self.addrs[key]

    async def get_all_addresses(self) -> Dict[str, Dict]:
        return self.addrs
~~~

In the `AddressBookService` class uses an in-memory dictionary to store the addresses. In reality, it will a lot more complicated, and using some databases. None the less, it is functioning. It is enough for implementing and testing the Web Framework layer.

---

## Tornado Web Framework

[Tornado](https://www.tornadoweb.org/){:target="_blank" rel="nofollow"} is a Python web framework with [asyncio](https://docs.python.org/3/library/asyncio.html){:target="_blank" rel="nofollow"} APIs (if needed, please review [asyncio cooperative multitasking concepts](/tech/python-microservices-01-tornado-asyncio-lint-test-coverage-project-setup.html){:target="_blank" rel="nofollow"}).

For implementing a service, you need to define following in Tornado:

- **Request Handlers** for endpoint methods,
- **Application** routing configuration mapping all request handlers to regex for endpoints,
- **HTTP Server** that listens on a given port and routes requests to the application.

### Request Handlers

A request handler is needed for every endpoint regex. For address-book service, there are two handlers needed:

- **AddressBookRequestHandler** for `/addresses/`: GET and POST methods for creating a new entry and listing all entries respectively,
- **AddressBookEntryRequestHandler** for `/addresses/{id}`: GET, PUT, and DELETE methods for reading, updating, and deleting a specific entry respectively.

~~~ python
# addrservice/tornado/app.py

class AddressBookRequestHandler(BaseRequestHandler):
    async def get(self):
        all_addrs = await self.service.get_all_addresses()
        self.set_status(200)
        self.finish(all_addrs)

    async def post(self):
        try:
            addr = json.loads(self.request.body.decode('utf-8'))
            id = await self.service.create_address(addr)
            addr_uri = ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(
                         id=id)
            self.set_status(201)
            self.set_header('Location', addr_uri)
            self.finish()
        except (json.decoder.JSONDecodeError, TypeError):
            raise tornado.web.HTTPError(
                400, reason='Invalid JSON body'
            ) from None
        except ValueError as e:
            raise tornado.web.HTTPError(400, reason=str(e))

class AddressBookEntryRequestHandler(BaseRequestHandler):
    async def get(self, id):
        try:
            addr = await self.service.get_address(id)
            self.set_status(200)
            self.finish(addr)
        except KeyError as e:
            raise tornado.web.HTTPError(404, reason=str(e))

    async def put(self, id):
        try:
            addr = json.loads(self.request.body.decode('utf-8'))
            await self.service.update_address(id, addr)
            self.set_status(204)
            self.finish()
        except (json.decoder.JSONDecodeError, TypeError):
            raise tornado.web.HTTPError(
                400, reason='Invalid JSON body'
            )
        except KeyError as e:
            raise tornado.web.HTTPError(404, reason=str(e))
        except ValueError as e:
            raise tornado.web.HTTPError(400, reason=str(e))

    async def delete(self, id):
        try:
            await self.service.delete_address(id)
            self.set_status(204)
            self.finish()
        except KeyError as e:
            raise tornado.web.HTTPError(404, reason=str(e))
~~~

Both of these inherit from `BaseRequestHandler` that has common functionalities. For example, Tornado returns HTTP response by default, but the address-book service must return JSON.

~~~ python
# addrservice/tornado/app.py

class BaseRequestHandler(tornado.web.RequestHandler):
    def initialize(
        self,
        service: AddressBookService,
        config: Dict
    ) -> None:
        self.service = service
        self.config = config

    def write_error(self, status_code: int, **kwargs: Any) -> None:
        self.set_header(
            'Content-Type', 'application/json; charset=UTF-8'
        )
        body = {
            'method': self.request.method,
            'uri': self.request.path,
            'code': status_code,
            'message': self._reason
        }
        if self.settings.get("serve_traceback") \
            and "exc_info" in kwargs:
            # in debug mode, send a traceback
            trace = '\n'.join(traceback.format_exception(
                *kwargs['exc_info']
            ))
            body['trace'] = trace
        self.finish(body)
~~~

The `BaseRequestHandler` utilizes the following Tornado hooks:

- **[write_error](https://www.tornadoweb.org/en/stable/web.html#tornado.web.RequestHandler.write_error){:target="_blank" rel="nofollow"} method:** to send a JSON error message instead of HTTP,
- **[serve_traceback](https://www.tornadoweb.org/en/stable/web.html#tornado.web.Application.settings){:target="_blank" rel="nofollow"} setting:** to send exception traceback in debug mode,
- **[initialize](https://www.tornadoweb.org/en/stable/web.html#tornado.web.RequestHandler.initialize){:target="_blank" rel="nofollow"} method:** to get the needed objects (like the underlying AddressBookService that has the business logic).

You will see how initialize and serve_traceback are tied to the handlers in the next section.

These handlers define a set of valid endpoint URLs. A default handler can be defined to handle all invalid URLs. The [`prepare`](https://www.tornadoweb.org/en/stable/web.html#tornado.web.RequestHandler.prepare){:target="_blank" rel="nofollow"} method is called for all HTTP methods.

~~~ python
# addrservice/tornado/app.py

class DefaultRequestHandler(BaseRequestHandler):
    def initialize(self, status_code, message):
        self.set_status(status_code, reason=message)

    def prepare(self) -> Optional[Awaitable[None]]:
        raise tornado.web.HTTPError(
            self._status_code, reason=self._reason
        )
~~~

### Application Routing Configuration

All request handlers need to be tied into a tornado.web.Application. That requires the following:

- **RegEx-handler mapping:** A list of a tuple (regex, handler class, parameters to handler's `initialize` method) that is how the service object is passed to all handlers,
- **Settings:** e.g., `serve_traceback`, `default_handler_class`.

~~~ python
# addrservice/tornado/app.py

ADDRESSBOOK_REGEX = r'/addresses/?'
ADDRESSBOOK_ENTRY_REGEX = r'/addresses/(?P<id>[a-zA-Z0-9-]+)/?'

def make_addrservice_app(
    config: Dict,
    debug: bool
) -> Tuple[AddressBookService, tornado.web.Application]:

    service = AddressBookService(config)

    app = tornado.web.Application(
        [
            (ADDRESSBOOK_REGEX,
                AddressBookRequestHandler,
                dict(service=service, config=config)),
            (ADDRESSBOOK_ENTRY_REGEX,
                AddressBookEntryRequestHandler,
                dict(service=service, config=config))
        ],
        compress_response=True,
        serve_traceback=debug,
        default_handler_class=DefaultRequestHandler,
        default_handler_args={
            'status_code': 404,
            'message': 'Unknown Endpoint'
        }
    )

    return service, app
~~~

The `make_addrservice_app` function creates an `AddressBookService` object, uses it to make `tornado.web.Application`, and then returns both the service and app.

In the debug mode, `serve_traceback` is set True. When an exception happens, the error returned to the client also has the exception string. We have found this very useful in debugging. Without requiring to scan through server logs and to attach a debugger to the server, the exception string at the client offers good pointers to the cause.

### HTTP Server

The application (that has routes to various request handlers) is started as an HTTP server with following steps:

- Start `AddressBook` (business logic) service
- Create an HTTP server ([`app.listen`](https://www.tornadoweb.org/en/stable/web.html#tornado.web.Application.listen){:target="_blank" rel="nofollow"})
- Start asyncio event loop ([`loop.run_forever`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_forever){:target="_blank" rel="nofollow"})

When the server is stopped, the server is stopped and all pending requests are completed:

- Stop asyncio event loop ([`loop.stop`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.stop){:target="_blank" rel="nofollow"})
- Stop HTTP server ([`http_server.stop`](https://www.tornadoweb.org/en/stable/tcpserver.html#tornado.tcpserver.TCPServer.stop){:target="_blank" rel="nofollow"})
- Complete all pending requests ([`loop.shutdown_asyncgens`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.shutdown_asyncgens){:target="_blank" rel="nofollow"})
- Stop `AddressBook` service
- Close the event loop ([`loop.close`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.close){:target="_blank" rel="nofollow"})


~~~ python
# addrservice/tornado/server.py

def run_server(
    app: tornado.web.Application,
    service: AddressBookService,
    config: Dict,
    port: int,
    debug: bool,
):
    name = config['service']['name']
    loop = asyncio.get_event_loop()

    service.start()  # Start AddressBook service (business logic)

    # Bind http server to port
    http_server_args = {
        'decompress_request': True
    }
    http_server = app.listen(port, '', **http_server_args)

    try:
        loop.run_forever()        # Start asyncio IO event loop
    except KeyboardInterrupt:
        # signal.SIGINT
        pass
    finally:
        loop.stop()               # Stop event loop
        http_server.stop()        # stop accepting new http reqs
        loop.run_until_complete(  # Complete all pending coroutines
            loop.shutdown_asyncgens()
        )
        service.stop()            # stop service
        loop.close()              # close the loop

def main(args=parse_args()):
    config = yaml.load(args.config.read(), Loader=yaml.SafeLoader)
    addr_service, addr_app = make_addrservice_app(
        config, args.debug
    )

    run_server(
        app=addr_app,
        service=addr_service,
        config=config,
        port=args.port,
        debug=args.debug,
    )
~~~

---

## The proof of the pudding

Let's run the server and try some requests.

### Run the server

~~~ bash
$ python3 addrservice/tornado/server.py --port 8080 --config ./configs/addressbook-local.yaml --debug

Starting Address Book on port 8080 ...
~~~

### Test default handler

There is no `/xyz` endpoint, so it returns 404:

~~~ bash
$ curl -i http://localhost:8080/xyz

HTTP/1.1 404 Unknown Endpoint
Server: TornadoServer/6.0.3
Content-Type: application/json; charset=UTF-8
Date: Tue, 10 Mar 2020 14:31:27 GMT
Content-Length: 518
Vary: Accept-Encoding

{"method": "GET", "uri": "/xyz", "code": 404, "message": "Unknown Endpoint", "trace": "Traceback (most recent call last):\n\n  File \"... redacted call stack trace ... addrservice/tornado/app.py\", line 67, in prepare\n    self._status_code, reason=self._reason\n\ntornado.web.HTTPError: HTTP 404: Unknown Endpoint\n"}
~~~

### Create an address entry

Add an address entry, the returned location is the id to query later:

~~~ bash
$ curl -i -X POST http://localhost:8080/addresses -d '{"full_name": "Bill Gates"}'

HTTP/1.1 201 Created
Server: TornadoServer/6.0.3
Content-Type: text/html; charset=UTF-8
Date: Tue, 10 Mar 2020 14:40:01 GMT
Location: /addresses/66fdbb78e79846849608b2cfe244a858
Content-Length: 0
Vary: Accept-Encoding
~~~

### Read the address entry

Use the `id` in the Location field in the previous request to query it:

~~~ bash
$ curl -i -X GET http://localhost:8080/addresses/66fdbb78e79846849608b2cfe244a858

HTTP/1.1 200 OK
Server: TornadoServer/6.0.3
Content-Type: application/json; charset=UTF-8
Date: Tue, 10 Mar 2020 14:44:26 GMT
Etag: "5496aee01a83cf2386641b2c43540fc5919d621e"
Content-Length: 22
Vary: Accept-Encoding

{"full_name": "Bill Gates"}
~~~

### Update the address entry

Let's change the name:

~~~ bash
$ curl -i -X PUT http://localhost:8080/addresses/66fdbb78e79846849608b2cfe244a858 -d '{"full_name": "William Henry Gates III"}'

HTTP/1.1 204 No Content
Server: TornadoServer/6.0.3
Date: Tue, 10 Mar 2020 14:48:04 GMT
Vary: Accept-Encoding
~~~

### List all addresses

~~~ bash
$ curl -i -X GET http://localhost:8080/addresses

HTTP/1.1 200 OK
Server: TornadoServer/6.0.3
Content-Type: application/json; charset=UTF-8
Date: Tue, 10 Mar 2020 14:49:10 GMT
Etag: "5601e676f3fa4447feaa8d2dd960be163af7570a"
Content-Length: 73
Vary: Accept-Encoding

{"66fdbb78e79846849608b2cfe244a858": {"full_name": "William Henry Gates III"}}
~~~

### Delete the address

~~~ bash
$ curl -i -X DELETE http://localhost:8080/addresses/66fdbb78e79846849608b2cfe244a858

HTTP/1.1 204 No Content
Server: TornadoServer/6.0.3
Date: Tue, 10 Mar 2020 14:50:38 GMT
Vary: Accept-Encoding
~~~

### Verify address is deleted

~~~ bash
$ curl -i -X GET http://localhost:8080/addresses

HTTP/1.1 200 OK
Server: TornadoServer/6.0.3
Content-Type: application/json; charset=UTF-8
Date: Tue, 10 Mar 2020 14:52:01 GMT
Etag: "bf21a9e8fbc5a3846fb05b4fa0859e0917b2202f"
Content-Length: 2
Vary: Accept-Encoding

{}

$ curl -i -X GET http://localhost:8080/addresses/66fdbb78e79846849608b2cfe244a858 

HTTP/1.1 404 '66fdbb78e79846849608b2cfe244a858'
Server: TornadoServer/6.0.3
Content-Type: application/json; charset=UTF-8
Date: Tue, 10 Mar 2020 14:53:06 GMT
Content-Length: 1071
Vary: Accept-Encoding

{"method": "GET", "uri": "/addresses/66fdbb78e79846849608b2cfe244a858", "code": 404, "message": "'66fdbb78e79846849608b2cfe244a858'", "trace": "Traceback (most recent call last):\n\n  File \"... redacted call stack trace ... addrservice/tornado/app.py\", line 100, in get\n    raise tornado.web.HTTPError(404, reason=str(e))\n\ntornado.web.HTTPError: HTTP 404: '66fdbb78e79846849608b2cfe244a858'\n"}
~~~

---

## Tornado Testing Framework

Manual testing is tedious and error-prone. Tornado provides [testing](https://www.tornadoweb.org/en/stable/testing.html){:target="_blank" rel="nofollow"} infrastructure. It starts the HTTP server and runs the tests. It does necessary plumbing to route the HTTP requests to the server it started.

Test classes should inherit from [`AsyncHTTPTestCase`](https://www.tornadoweb.org/en/stable/testing.html#tornado.testing.AsyncHTTPTestCase){:target="_blank" rel="nofollow"}, and implement a [`get_app`](https://www.tornadoweb.org/en/stable/testing.html#tornado.testing.AsyncHTTPTestCase.get_app){:target="_blank" rel="nofollow"} method, which returns the `tornado.web.Application`. It is similar to what is done in `server.py`. Code duplication can be kept at a minimum by reusing `make_addrservice_app` function in `get_app`.

Tornado creates a new `IOLoop` for each test. When it is not appropriate to use a new loop, you should override [`get_new_ioloop`](https://www.tornadoweb.org/en/stable/testing.html#tornado.testing.AsyncTestCase.get_new_ioloop){:target="_blank" rel="nofollow"} method.

~~~ python
# tests/unit/tornado_app_handlers_test.py

class AddressServiceTornadoAppTestSetup(
    tornado.testing.AsyncHTTPTestCase
):
    def get_app(self) -> tornado.web.Application:
        addr_service, app = make_addrservice_app(
            config=TEST_CONFIG,
            debug=True
        )

        addr_service.start()
        atexit.register(lambda: addr_service.stop())

        return app

    def get_new_ioloop(self):
        return IOLoop.current()
~~~

### Unit Test Cases

For address book service, except default handler, all handlers use the service (business logic) module. That module has only simple stubs in this blog post, but in reality, it will be way more complex. So only default handler is independent and qualifies for the unit tests. All other handlers should be covered in the integration tests (next section).

~~~ python
# tests/unit/tornado_app_handlers_test.py

class AddressServiceTornadoAppUnitTests(
    AddressServiceTornadoAppTestSetup
):
    def test_default_handler(self):
        r = self.fetch(
            '/does-not-exist',
            method='GET',
            headers=None,
        )
        info = json.loads(r.body.decode('utf-8'))

        self.assertEqual(r.code, 404, info)
        self.assertEqual(info['code'], 404)
        self.assertEqual(info['message'], 'Unknown Endpoint')
~~~

### Integration Test Cases

The whole life cycle of an address entry tested manually earlier can be automated as integration tests. It will be a lot easier and faster to run all those tests in seconds every time you make a code change.

~~~ python
# tests/integration/tornado_app_addreservice_handlers_test.py

ADDRESSBOOK_ENTRY_URI_FORMAT_STR = r'/addresses/{id}'

class TestAddressServiceApp(AddressServiceTornadoAppTestSetup):
    def test_address_book_endpoints(self):
        # Get all addresses in the address book, must be ZERO
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id=''),
            method='GET',
            headers=None,
        )
        all_addrs = json.loads(r.body.decode('utf-8'))
        self.assertEqual(r.code, 200, all_addrs)
        self.assertEqual(len(all_addrs), 0, all_addrs)

        # Add an address
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id=''),
            method='POST',
            headers=self.headers,
            body=json.dumps(self.addr0),
        )
        self.assertEqual(r.code, 201)
        addr_uri = r.headers['Location']

        # POST: error cases
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id=''),
            method='POST',
            headers=self.headers,
            body='it is not json',
        )
        self.assertEqual(r.code, 400)
        self.assertEqual(r.reason, 'Invalid JSON body')

        # Get the added address
        r = self.fetch(
            addr_uri,
            method='GET',
            headers=None,
        )
        self.assertEqual(r.code, 200)
        self.assertEqual(self.addr0, json.loads(r.body.decode('utf-8')))

        # GET: error cases
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id='no-such-id'),
            method='GET',
            headers=None,
        )
        self.assertEqual(r.code, 404)

        # Update that address
        r = self.fetch(
            addr_uri,
            method='PUT',
            headers=self.headers,
            body=json.dumps(self.addr1),
        )
        self.assertEqual(r.code, 204)
        r = self.fetch(
            addr_uri,
            method='GET',
            headers=None,
        )
        self.assertEqual(r.code, 200)
        self.assertEqual(self.addr1, json.loads(r.body.decode('utf-8')))

        # PUT: error cases
        r = self.fetch(
            addr_uri,
            method='PUT',
            headers=self.headers,
            body='it is not json',
        )
        self.assertEqual(r.code, 400)
        self.assertEqual(r.reason, 'Invalid JSON body')
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id='1234'),
            method='PUT',
            headers=self.headers,
            body=json.dumps(self.addr1),
        )
        self.assertEqual(r.code, 404)

        # Delete that address
        r = self.fetch(
            addr_uri,
            method='DELETE',
            headers=None,
        )
        self.assertEqual(r.code, 204)
        r = self.fetch(
            addr_uri,
            method='GET',
            headers=None,
        )
        self.assertEqual(r.code, 404)

        # DELETE: error cases
        r = self.fetch(
            addr_uri,
            method='DELETE',
            headers=None,
        )
        self.assertEqual(r.code, 404)

        # Get all addresses in the address book, must be ZERO
        r = self.fetch(
            ADDRESSBOOK_ENTRY_URI_FORMAT_STR.format(id=''),
            method='GET',
            headers=None,
        )
        all_addrs = json.loads(r.body.decode('utf-8'))
        self.assertEqual(r.code, 200, all_addrs)
        self.assertEqual(len(all_addrs), 0, all_addrs)
~~~

### Code Coverage

Let's run these tests:

~~~ bash
# All tests
$ ./run.py test

# Only unit tests
$ ./run.py test --suite unit

# Only integration tests
$ ./run.py test --suite integration
~~~

Let's check code coverage:

~~~ bash
$ coverage run --source=addrservice \
    --omit="addrservice/tornado/server.py" \
    --branch ./run.py test

$ coverage report
Name                              Stmts   Miss Branch BrPart  Cover
-------------------------------------------------------------------
addrservice/__init__.py               2      0      0      0   100%
addrservice/service.py               23      1      0      0    96%
addrservice/tornado/__init__.py       0      0      0      0   100%
addrservice/tornado/app.py           83      4      8      3    92%
-------------------------------------------------------------------
TOTAL                               108      5      8      3    93%
~~~

As you can see, it is pretty good coverage.

Notice that `addrservice/tornado/server.py` was omitted from code coverage. It has the code that runs the HTTP server, but Tornado test infra has its own mechanism of running the HTTP server. This is the only file that can not be covered by unit and integration tests. Including it will skew the overall coverage metrics.

~~~ bash
$ coverage run --source=addrservice --branch ./run.py test

$ coverage report
Name                              Stmts   Miss Branch BrPart  Cover
-------------------------------------------------------------------
addrservice/__init__.py               2      0      0      0   100%
addrservice/service.py               23      1      0      0    96%
addrservice/tornado/__init__.py       0      0      0      0   100%
addrservice/tornado/app.py           83      4      8      3    92%
addrservice/tornado/server.py        41     41      2      0     0%
-------------------------------------------------------------------
TOTAL                               149     46     10      3    68%
~~~

---

## Summary

In this article, you learned about how to put together a microservice and tests using Tornado:

- **Layered design:** Isolate endpoint code in Web Framework Layer, and implement business logic in Service Layer.
- **Tornado:** Implement the web framework layer with Tornado request handlers, app endpoint routing, and HTTP server.
- **Tests:** Write unit and integration tests for the web framework layer using Tornado testing infrastructure.
- **Tooling:** Use lint, test, code coverage for measuring the health of the code. Integrate early, write stub code if necessary to make it run end-to-end.
