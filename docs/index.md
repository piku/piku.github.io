# Welcome

`piku`, inspired by [dokku][dokku], allows you do `git push` deployments to your own servers, no matter how small they are.

![asciicast](img/demo.svg)

## Workflow

`piku` supports a Heroku-like workflow:

* Create a `git` SSH remote pointing to your `piku` server with the app name as repo name:
  `git remote add piku piku@yourserver:appname`.
* Push your code: `git push piku master` (or if you want to push a different branch than the current one use `git push piku release-branch-name`).
* `piku` determines the runtime and installs the dependencies for your app (building whatever's required).
* It then looks at a [`Procfile`](configuration/procfile.md) and starts matching worker processes.
  * You can optionally also specify a `release` worker which is run once when the app is deployed.
* You can then remotely change application settings (`config:set`) or scale up/down worker processes (`ps:scale`).
* You can also bake application and `nginx` settings into an [`ENV` configuration file](configuration/index.md#configuring-piku-via-env). 

You can also deploy a `gh-pages` style static site using a `static` worker type, with the root path as the argument, and run a `release` task to do some processing on the server after `git push`.

## Project Activity

**`piku` is considered STABLE**. It is actively maintained, but "actively" here means the feature set is pretty much done, so it is only updated when new language runtimes are added or reproducible bugs crop up.

It is currently being refactored to require Python 3.7 or above, since even though 3.8+ is now the baseline Python 3 version in Ubuntu LTS 20.04 and Debian 11 has already moved on to 3.9, there are no substantial differences between those versions.

## Deprecation Notices

Since most of its users run it on LTS distributions, there is no rush to introduce disruption. The current plan is to throw up a warning for older runtimes and do regression testing for 3.7, 3.8, 3.9 and 3.10 (replacing the current bracket of tests from 3.5 to 3.8), and make sure we also cover Ubuntu 22.04, Debian 11 and Fedora 37+.


## Install

`piku` can manage multiple apps on a single machine, and all you need is a VPS, Raspberry Pi, or other server.

There are two main ways of deploying `piku` onto a new server:

* Use [`piku-bootstrap`](https://github.com/piku/piku-bootstrap) to reconfigure a new or existing Ubuntu virtual machine.
* Use `cloud-init` when creating a new virtual machine or barebones automated deployment (check [this repository](https://github.com/piku/cloud-init) for examples).

## Manage - via the `piku` helper

To make life easier you can also install the [piku](./piku) helper into your path (e.g. `~/bin`).

```shell
curl https://raw.githubusercontent.com/piku/piku/master/piku > ~/bin/piku && chmod 755 ~/bin/piku
```

This shell script simplifies working with multiple `piku` remotes and applications:

* If you `cd` into a project folder that has a `git` remote called `piku` the helper will infer the remote server and app name and use them automatically:

```shell
$ piku logs
$ piku config:set MYVAR=12
$ piku stop
$ piku deploy
$ piku destroy
$ piku # <- show available remote and local commands
```

* If you are starting a new project, `piku init` will download example `Procfile` and `ENV` files into the current folder:

```shell
$ piku init
Wrote ./ENV file.
Wrote ./Procfile.
```

* The `piku` helper also lets you pass settings to the underlying SSH command: `-t` to run interactive commands remotely, and `-A` to proxy authentication credentials in order to do remote `git` pulls.

For instance, here's how to use the `-t` flag to obtain a `bash` shell in the app directory of one of your `piku` apps:

```shell
$ piku -t run bash
Piku remote operator.
Server: piku@cloud.mccormickit.com
App: dashboard

piku@piku:~/.piku/apps/dashboard$ ls
data  ENV  index.html  package.json  package-lock.json  Procfile  server.wisp
```

[dokku]: https://github.com/dokku/dokku
[raspi-cluster]: https://github.com/rcarmo/raspi-cluster
[cygwin]: http://www.cygwin.com
[uwsgi]: https://github.com/unbit/uwsgi
[wsl]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux

