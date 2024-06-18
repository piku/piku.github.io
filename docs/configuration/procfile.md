
# `Procfile` format

`piku` supports a Heroku-like `Procfile` that you provide to indicate how to run one or more application processes (what Heroku calls "dynos"):

```bash
web: embedded_server --port $PORT
worker: background_worker
```

## Worker Types

`piku` supports six different kinds of worker processes:

```bash
# A module to be loaded by uwsgi to serve HTTP requests
wsgi: module.submodule:app
# A background worker, using the default name
worker: python long_running_script.py
# Another worker with a different name
fetcher: python fetcher.py
# Simple cron expression: minute [0-59], hour [0-23], day [0-31], month [1-12], weekday [1-7] (starting Monday, no ranges allowed on any field)
cron: 0 0 * * * python midnight_cleanup.py
release: python initial_cleanup.py
```
Each of these has slightly different features:

### `wsgi`

`wsgi` workers are Python-specific and must be specified in the format `dotted.module:entry_point`. `uwsgi` will load the specified module and call the `entry_point` function to start the application, handling all the HTTP requests directly (your Python code will run the handlers, but will run as a part of the `uwsgi` process).

`uwsgi` will automatically spawn multiple workers for you, and you can control the number of workers via the `UWSGI_PROCESSES` environment variable.

Also, in this mode `uwsgi` will talk to `nginx` via a Unix socket, so you don't need to worry about the HTTP server at all.

### `web`

`web` workers can be literally any executable that `uwsgi` can run and that will serve HTTP requests. They must (by convention) honor the `PORT` environment variable, so that the `nginx` reverse proxy can talk to them.

### `worker`

`worker` processes are just standard processes that run in the background. They can actually have arbitrary names, and the idea is that they would perform any tasks your app requires that isn't directly related to serving web pages.

### `static`

`static` workers are simply a way to direct `nginx` to mount the first argument as a static path and serve it directly. This is useful for serving (and caching) static files directly from your app, without having to go through application code.

!!! note
    See [`nginx` caching](index.md#nginx-caching) for more information on how to configure `nginx` to serve static files.

### `cron`

A `cron` worker is a process that runs at a specific time (or intervals), using a simplified `crontab` expression preceding the command to be run (e.g. `cron: */5 * * * * python batch.py` to run a batch every 5 minutes)

!!! warning
    `crontab` expressions are simplified and do not support ranges or lists, only single values, splits and `*` (wildcard).

#### alternatives
```Python```
[apscheduler](https://github.com/agronholm/apscheduler) - Provides it's own library for scheduling jobs, and honors the full regex of crontab.


### `preflight`

`preflight`  is a special "worker" that is run once _before_ the app is deployed _and_ dependencies are installed (can be useful for cleanups, like resetting caches, removing older versions of files, etc).

### `release`

`release` which is a special worker that is run once when the app is deployed, _after_ installing dependencies (can be useful for build steps).



Any worker will be automatically respawned upon failure ([uWSGI][uwsgi] will automatically shun/throttle crashy workers).
