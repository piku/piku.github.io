
# `Procfile` format

`piku` recognizes six kinds of process declarations in the `Procfile`:

* `wsgi` workers, in the format `dotted.module:entry_point` (Python-only)
* `web` workers, which can be anything that honors the `PORT` environment variable
* `static` workers, which simply mount the first argument as the root static path
* `preflight` which is a special "worker" that is run once _before_ the app is deployed _and_ installing deps (can be useful for cleanups).
* `release` which is a special worker that is run once when the app is deployed, after installing deps (can be useful for build steps).
* `cron` workers, which require a simplified `cron` expression preceding the command to be run (e.g. `cron: */5 * * * * python batch.py` to run a batch every 5 minutes)
* `worker` processes, which are standalone workers and can have arbitrary names

So a Python application could have a `Procfile` like such:

```bash
# A module to be loaded by uwsgi to serve HTTP requests
wsgi: module.submodule:app
# A background worker
worker: python long_running_script.py
# Another worker with a different name
fetcher: python fetcher.py
# Simple cron expression: minute [0-59], hour [0-23], day [0-31], month [1-12], weekday [1-7] (starting Monday, no ranges allowed on any field)
cron: 0 0 * * * python midnight_cleanup.py
release: python initial_cleanup.py
```

...whereas a generic app would be:

```bash
web: embedded_server --port $PORT
worker: background_worker
```

Any worker will be automatically respawned upon failure ([uWSGI][uwsgi] will automatically shun/throttle crashy workers).
