# Community

## [LinuxConf Talk](https://www.youtube.com/watch?v=ec-GoDukHWk)


<iframe width="560" height="315" src="https://www.youtube.com/embed/ec-GoDukHWk?si=lfXqRJSjia8ZH7YM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## References

* [Fast Web App Tutorial](https://github.com/piku/webapp-tutorial)

## Contribution Guidelines

- Small and focused PRs. Please don't include changes that don't address the subject of your PR.
- Follow the style of importing functions directly e.g. `from os.path import abspath`
- Follow `PEP8`.

### Core values

* Must run on low end devices.
* Accessible to hobbyists and K-12 schools.
* ~1500 lines readable code.
* Functional code style.
* Few (single?) dependencies
* [12 factor app](https://12factor.net).
* Simplify user experience.
* Cover 80% of common use cases.
* Sensible defaults for all features.
* Leverage distro packages in Raspbian/Debian/Ubuntu (Alpine and RHEL support is WIP)
* Leverage standard tooling (`git`, `ssh`, `uwsgi`, `nginx`).
* Preserve backwards compatibility where possible