# Configuring Applications

A minimal `piku` app has a root directory structure similar to  this:

```bash
ENV
Procfile
app.py
worker.py
requirements.txt
```

### Configuration Files

`piku` relies on two configuration files shipped with your app to determine how to run it: [`Procfile`](procfile.md) and [`ENV`](env.md).

* The [`Procfile`](procfile.md) tells `piku` what kind of workers to run
* The [`ENV`](env.md) file contains environment variables that allow you to configure both `piku` and your app, following the [12-factor app](https://12factor.net) approach.

### Runtime Detection

Besides [`ENV`](env.md) and [`Procfile`](procfile.md), `piku` also looks for runtime-specific files in the root of your app's directory:

* If there's a `requirements.txt` file at the top level, then the app is assumed to require Python.
* If there's a `Gemfile` at the top level, then the app is assumed to require Ruby.
* If there's a `package.json` file at the top level, then the app is assumed to require Node.js.
* If there's a `pom.xml` or a `build.gradle` file at the top level, then the app is assumed to require Java.
* If there's a `deps.edn` or `project.clj` file at the top level, then the app is assumed to require Clojure.
* If there's a `Godeps` file at the top level, then the app is assumed to require Go.

!!! info
    `go.mod` support is currently in development.

These are not exclusive, however. There is also [a sample Phoenix app](../community/samples.md#phoenix) that demonstrates how to add support for additional runtimes.