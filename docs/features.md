# Features

## Virtual Hosts and SSL

`piku` has full virtual host support - i.e., you can host multiple apps on the same VPS and use DNS aliases to access them via different hostnames. 

`piku`  will also set up either a private certificate or obtain one via [Let's Encrypt](https://letsencrypt.org/) to enable SSL.

If you are on a LAN and are accessing `piku` from macOS/iOS/Linux clients, you can try using [`piku/avahi-aliases`](https://github.com/piku/avahi-aliases) to announce different hosts for the same IP address via Avahi/mDNS/Bonjour.

## Caching and Static Paths

Besides static sites, `piku` also supports directly mapping specific URL prefixes to filesystem paths (to serve static assets) or caching back-end responses (to remove load from applications).

These features are configured by setting appropriate values in the [`ENV`](ENV.md) file.

## Supported Platforms

`piku` is intended to work in any POSIX-like environment where you have Python, `nginx`, [`uWSGI`][uwsgi] and SSH: it has been deployed on Linux, FreeBSD, [Cygwin][cygwin] and the [Windows Subsystem for Linux][wsl].

As a baseline, it began its development on an original 256MB Rasbperry Pi Model B, and still runs reliably on it.

But its main use is as a micro-PaaS to run applications on cloud servers with both Intel and ARM CPUs, with Debian and Ubuntu Linux as target platforms.

## Supported Runtimes

`piku` currently supports apps written in Python, Node, Clojure, Java and a few other languages (like Go) in the works.

But as a general rule, if it can be invoked from a shell, it can be run inside `piku`.