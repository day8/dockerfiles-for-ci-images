# Dockerfiles for `day8/core` and Derivative Images

The [`ghcr.io/day8/core`][1] and derivative Docker images are the 
reference testing environment used throughout [Day8's](https://www.day8.com.au/) 
continuous integration pipeline.

On pushing code to GitHub, this Docker image is the environment used to execute tests and build 
releases for deployment with GitHub Actions.
 
This repository contains the [`Dockerfile`s][3] to build the [`ghcr.io/day8/core`][1] 
Docker image. It also contains the [GitHub Actions][4] that test and deploy the Docker image.

[Ubuntu 20.04 LTS][5] was chosen as the base image as it is a long term stable release of a widely 
understood and supported distribution.

[1]: https://github.com/orgs/day8/packages?repo_name=dockerfiles-for-dev-ci-images
[2]: https://www.day8.com.au/
[3]: https://github.com/day8/dockerfiles-for-dev-ci-images/blob/master/Dockerfile
[4]: https://github.com/day8/dockerfiles-for-dev-ci-images/actions
[5]: https://hub.docker.com/_/ubuntu

## Quick Start

To run an interactive terminal:
```
$ docker login ghcr.io
$ docker run -it --rm ghcr.io/day8/core:5.0.0
```

## Build Requirements

On image startup the exact versions of important software are printed to the
console.

| Name                                                | Version               | Description | Origin | 
| --------------------------------------------------- |-----------------------| ----------- | ------ |
| Leiningen                                           | `2.9.x`               | Clojure(Script) build tool. Day8's main build tool. | [GitHub Releases Assets](https://github.com/technomancy/leiningen/releases) |
| Clojure                                             | `1.11.x`              | 'Official' Clojure CLI tools. | [Clojure Website](https://clojure.org/guides/getting_started) |
| OpenJDK                                             | `11.x` (LTS)          | Java runtime. Dependency of Leiningen, `clojure` CLI etc. | [Ubuntu Package: `openjdk-11-headless`](https://packages.ubuntu.com/focal-updates/openjdk-11-jdk-headless) |
| Node.js                                             | `18.12.x` (LTS)       | JavaScript runtime. Dependency of `shadow-cljs`. | [NodeSource Package Repository](https://github.com/nodesource/distributions) |
| NPM                                                 | `9.1.x` (LTS)         | JavaScript package manager. Dependency of `shadow-cljs`. | Bundled with Node.js |
| Python 2                                            | `2.7.x`               | Python 2 runtime. | [Ubuntu Package: `python2`](https://packages.ubuntu.com/focal/python2) |
| Python 3                                            | `3.8.x`               | Python 3 runtime. | [Ubuntu Package: `python3`](https://packages.ubuntu.com/focal/python3) |
| `pip`                                               | Latest at build time. | Python package manager. | [Ubuntu Package: `python3-pip`](https://packages.ubuntu.com/focal/python3-pip) |
| `pipenv`                                            | Latest at build time. | Python package manager. | [Python Package: `pipenv`](https://pypi.org/project/pipenv/) |
| `pytest`                                            | Latest at build time. | Python test runner. | [Python Package: `pytest`](https://pypi.org/project/pytest/) |
| Git                                                 | Latest at build time. | Dependency of [`actions/checkout`](https://github.com/actions/checkout) and [`day8/lein-git-inject`](https://github.com/day8/lein-git-inject) | ['Git stable releases' Ubuntu PPA](https://launchpad.net/~git-core/+archive/ubuntu/ppa) |
| [`aws`](https://docs.aws.amazon.com/cli/index.html) | Latest at build time. | Interface to Amazon Web Services. Dependency of S3 deployments. | [Official AWS Package: `awscli-exe-linux-x86_64.zip`](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html) |
| [`sam`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) |                       | Creates and manages AWS serverless applications. | [Official AWS Package: `aws-sam-cli-linux-x86_64.zip`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install-linux.html) |
| GNU Compiler Collection                             | `9.3.x`               | C (`gcc`) and C++ (`g++`) compiler. Dependency of `npm install...` and therefore `shadow-cljs`. | [Ubuntu Package: `build-essential`](https://packages.ubuntu.com/focal/build-essential) |
| `make`                                              | Latest at build time. | Build automation tool, esp common for older C/C++ projects. Dependency of space-vim. | [Ubuntu Package: `build-essential`](https://packages.ubuntu.com/focal/build-essential) |
| `cmake`                                             | Latest at build time. | Build automation tool, esp common for newer C/C++ projects. |  [Ubuntu Package: `cmake`](https://packages.ubuntu.com/focal/cmake) |
| `zstd`                                              | Latest at build time. | Fast, lossless compression. Dependency of [`actions/cache@v2`](https://github.com/actions/cache). | [Ubuntu Package: `zstd`](https://packages.ubuntu.com/focal/zstd) |
| `Xvfb`                                              | Latest at build time. | X virtual framebuffer. Dependency of running Chrome without a display in GitHub Actions. | [Ubuntu Package: `xvfb`](https://packages.ubuntu.com/focal/xvfb) |
| `karma` CLI                                         | `2.0.0`               | Dependency of builds that use the [Karma test runner](https://karma-runner.github.io/latest/index.html). | [npm: `karma-cli`](https://www.npmjs.com/package/karma-cli) |
| Chromium                                            | `56.0.2924.0`         | Used as a browser to execute Karma tests. This specific old version tracks the version of [Electron](https://www.electronjs.org/) that Day8 has deployed. | [Long storey](https://github.com/day8/dockerfiles-for-dev-ci-images/blob/5dd8bbc8032f9ed17fd75da378e4d03a3c00cd5b/Dockerfile#L334)  |
| ChromeDriver                                        | `2.29`                | Dependency of executing Karma tests in Chromium. | [Project Website](https://chromedriver.chromium.org/) |

## REPLs

The following [REPLs](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) are
available for exploratory programming and debugging of code snippets.

| Name               | Language        | Example                                       |
| ------------------ | --------------- | --------------------------------------------- | 
| 'Official' Clojure | Clojure         | `docker run -it --rm ghcr.io/day8/core:2 clojure`   |
| Leiningen          | Clojure         | `docker run -it --rm ghcr.io/day8/core:2 lein repl` |
| Node.js            | JavaScript      | `docker run -it --rm ghcr.io/day8/core:2 node`      |
| Python 2           | Python 2        | `docker run -it --rm ghcr.io/day8/core:2 python2`   |
| Python 3           | Python 3        | `docker run -it --rm ghcr.io/day8/core:2 python3`   |
| Bash               | Bash            | `docker run -it --rm ghcr.io/day8/core:2 bash`      |

## Command-Line Tools

The following tools are not usually required for builds (e.g. GitHub Actions). These are included for convenience and
usability when using the image as an interactive shell.

| Tool                              | Description | Origin |
| --------------------------------- | --- | --- |
| [`clj-kondo`](https://github.com/clj-kondo/clj-kondo) | A linter for Clojure code that sparks joy. | GitHub Releases Assets |
| [`babashka`](https://github.com/babashka/babashka) | Native Clojure interpreter for scripting. | GitHub Releases Assets |
| [`gh`](https://github.com/cli/cli/) | GitHub's official command line tool. | [GitHub Releases Assets: `gh_N.N.N_linux_amd64.deb`](https://github.com/cli/cli/releases) |
| [`ssh`](https://www.openssh.com/) | OpenSSH client. | [Ubuntu Package: `openssh-client`](https://packages.ubuntu.com/focal/openssh-client) |

## Why Not Docker Hub ?

In 2020 [Docker Hub introduced rate limiting](https://www.docker.com/blog/scaling-docker-to-serve-millions-more-developers-network-egress/)
and around the same time [GitHub Actions finally fixed use of GitHub Container Registry](https://github.blog/changelog/2020-09-24-github-actions-private-registry-support-for-job-and-service-containers/).

Therefore we no longer publish images to Docker Hub and use GitHub Container Registry instead.

## GitHub Actions Example

```yaml
jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/day8/chrome-latest:5.0.0
      credentials:
        username: ${{ github.actor }}
        password: ${{ secrets.GLOBAL_TOKEN_FOR_GITHUB }} # <-- you need to create a GitHub Secret with a manual token that has global access as github.token only has access to the current repo! 
```

## Troubleshooting

### Upgrading Chrome / Chromium

#### Problem

The `day8/chrome-87:6.0.0` Docker image is provided as we need to test against a version equivalent to an old Electron version.

When the version of Chromium used needs to be upgraded to match an Electron upgrade.

#### Solution

Hold on to your hat and [walk this path](https://github.com/day8/dockerfiles-for-dev-ci-images/blob/5dd8bbc8032f9ed17fd75da378e4d03a3c00cd5b/Dockerfile#L334).

### Karma - 'Gtk: cannot open display: :99'

#### Problem

When executing Karma tests an error similar to the following is displayed:
```
21 08 2020 12:10:13.052:ERROR [launcher]: ChromeHeadless stdout:
21 08 2020 12:10:13.052:ERROR [launcher]: ChromeHeadless stderr: [0821/121012:ERROR:nacl_helper_linux.cc(311)] NaCl helper process running without a sandbox!
Most likely you need to configure your SUID sandbox correctly
[2442:2442:0821/121013:ERROR:browser_main_loop.cc(272)] Gtk: cannot open display: :99
```

#### Solution

`xvfb`, the X virtual framebuffer, is not running. This should be started by the 
`/docker-entrypoint.sh` script in the container which is the `ENTRYPOINT` of the Docker image. 

If this error is occurring on GitHub Actions it may be because:
1. GitHub Actions overrides the Docker image `ENTRYPOINT` with the `--entrypoint` CLI option to be 
   `/usr/bin/tail`.
2. We replace `/usr/bin/tail` with an intercept script that executes `/docker-entrypoint.sh` before 
   executing the original tail at `/usr/bin/tail.original`.
3. GitHub Actions has changed the `--entrypoint` option to something other than `/usr/bin/tail`.

## Deployment

Simply push a semver tag of the form `v1.2.3`. GitHub Actions will publish Docker images for 
`day8/core:1.2.3`, `day8/core:1.2` and `day8/core:1`. E.g.:

```shell
$ git tag v1.2.3 HEAD
$ git push --tags
```

## License

This repository is [MIT licenced](license.txt)
