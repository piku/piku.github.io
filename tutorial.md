# Tutorial

> This is a draft that we'll need to turn into HTML via the least possibly invasive `gh-pages` engine (TBD)

## Basic Concepts

`piku` builds upon three main components which are available across most Linux distributions:

* `nginx` - the web server engine, which exposes your application via the standard HTTP ports and handles (optional) encryption for secure sites
* `uwsgi` - the supervisor and application runner, which restarts your application when it dies but will also actually _load and run it_ inside its own process 
* `incron` - which essentially lets the others know `piku` has deployed or changed your app configuration

Why is this important to know? Well, because how your application talks to the outside world depends on how these work together with your code.

> This tutorial assumes you're familiar with `git` and the UNIX terminal, and that you can access the machine running `piku` either via its IP address or via its hostname.


## Your First Application

Let's start with the basics. Create a new folder and intialize a new repository on it:

```bash
mkdir piku-tests
cd piku-tests
git init .
```

Now add an `index.html` file:

```html
<html>
    <head>
        <title>piku</title>
    </head>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
```

... and a `server.py` file:

```python
#! /bin/env python3
import os
import http.server
import socketserver

PORT = os.environ.get("PORT", 8080)
BIND_ADDRESS = os.environ.get("BIND_ADDRESS", "")

with socketserver.TCPServer(
        (BIND_ADDRESS, PORT), 
        http.server.SimpleHTTPRequestHandler
    ) as httpd:
    print(f"serving at {BIND_ADDRESS}:{PORT}")
    httpd.serve_forever()
```

Note that we're using Python 3 - that is the default Python version in post-2020, although `piku` can be persuaded to use older versions (and other languages).

The important thing about this app is that _it is its own web server_, which is a fairly common case (but, as we'll see, not the most efficient one). It will look up the `BIND_ADDRESS` and `PORT` variables, open a socket (on every IP address if `BIND_ADDRESS` is empty) and listen for HTTP requests on `PORT`. And since it is a well behaved app, it will get those values from the environment, with sensible defaults.

Now let's add an ENV file to set those values, and a few other (optional, but useful) ones are fairly typical:

```bash
BIND_ADDRESS=0.0.0.0
PORT=8000
LC_ALL=en_US.UTF-8
LANG=$LC_ALL
PYTHONIOENCODING=UTF_8:replace
TZ=Europe/Lisbon
```

...and let's let `piku` know how to run our app by adding a `Procfile`:

```
# the -u option sets unbuffered output, which is essential when using print for logs
web: python -u server.py
```

This tells `piku` that our application has a standalone web server that is _not_ fronted by `nginx`, 


## Finding Your Server


> Topics to cover

* IP
* Hostname (LAN and Public DNS)
* Bonjour


## Binding to Localhost

## Using Port 80 

## Using `uwsgi`

## Configuring TLS
