# Dockerfile for `day8au/dev-ci` Image

The [`docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci`][1] Docker image is the 
reference development and testing environment used throughout [Day8's](https://www.day8.com.au/) 
development pipeline.

There are two main use cases for the [`docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci`][1] Docker image:
1. During development [Okteto](https://okteto.com/) is used to run this Docker image in Kubernetes.
2. On pushing code to GitHub, this Docker image is the environment used to execute tests and build 
   releases for deployment with GitHub Actions.
 
This repository contains the [`Dockerfile`][3] to build the [`docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci`][1] 
Docker image. It also contains the [GitHub Actions][4] that test and deploy the Docker image.

[Ubuntu 20.04 LTS][5] was chosen as the base image as it is a long term stable release of a widely 
understood and supported distribution.

[1]: https://github.com/orgs/day8/packages?repo_name=dockerfile-for-dev-ci-image
[2]: https://www.day8.com.au/
[3]: https://github.com/day8/dockerfile-for-dev-ci-image/blob/master/Dockerfile
[4]: https://github.com/day8/dockerfile-for-dev-ci-image/actions
[5]: https://hub.docker.com/_/ubuntu

## Quick Start

To run an interactive terminal:
```
$ docker login docker.pkg.github.com
$ docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0.1
```

## Build Requirements

On image startup the exact versions of important software are printed to the
console.

| Name                                                | Version               | Description | Origin | 
| --------------------------------------------------- | --------------------- | ----------- | ------ |
| Leiningen                                           | `2.9.x`               | Clojure(Script) build tool. Day8's main build tool. | [GitHub Releases Assets](https://github.com/technomancy/leiningen/releases) |
| Clojure                                             | `1.10.x`              | 'Official' Clojure CLI tools. | [Clojure Website](https://clojure.org/guides/getting_started) |
| Boot                                                |                       | Alternative to Leiningen. Day8 does not use it but several important 3rd party projects do use it. | [GitHub Release Assets](https://github.com/boot-clj/boot-bin/releases) |
| OpenJDK                                             | `11.x` (LTS)          | Java runtime. Dependency of Leiningen, `clojure` CLI, Boot etc. | [Ubuntu Package: `openjdk-11-headless`](https://packages.ubuntu.com/focal-updates/openjdk-11-jdk-headless) |
| Node.js                                             | `12.x` (LTS)          | JavaScript runtime. Dependency of `shadow-cljs`, `lumo`. | [NodeSource Package Repository](https://github.com/nodesource/distributions) |
| NPM                                                 | `6.x` (LTS)           | JavaScript package manager. Dependency of `shadow-cljs`. | Bundled with Node.js |
| Yarn                                                | `1.x` ('Classic')     | JavaScript package manager. Alternative to `npm`. | [Yarn Package Repository](https://classic.yarnpkg.com/en/docs/install#debian-stable) |
| Python 2                                            | `2.7.x`               | Python 2 runtime. | [Ubuntu Package: `python2`](https://packages.ubuntu.com/focal/python2) |
| Python 3                                            | `3.8.x`               | Python 3 runtime. | [Ubuntu Package: `python3`](https://packages.ubuntu.com/focal/python3) |
| `pip`                                               | Latest at build time. | Python package manager. | [Ubuntu Package: `python3-pip`](https://packages.ubuntu.com/focal/python3-pip) |
| `pipenv`                                            | Latest at build time. | Python package manager. | [Python Package: `pipenv`](https://pypi.org/project/pipenv/) |
| `pytest`                                            | Latest at build time. | Python test runner. | [Python Package: `pytest`](https://pypi.org/project/pytest/) |
| `flake8`                                            | Latest at build time. | Python source code checker/linter. | [Python Package: `flake8`](https://pypi.org/project/flake8/) |
| Git                                                 | Latest at build time. | Dependency of [`actions/checkout`](https://github.com/actions/checkout) and [`day8/lein-git-inject`](https://github.com/day8/lein-git-inject) | ['Git stable releases' Ubuntu PPA](https://launchpad.net/~git-core/+archive/ubuntu/ppa) |
| Git LFS                                             | Latest at build time. | Required to clone Git repositories using Large File Storage (LFS). | [PackageCloud](https://packagecloud.io/github/git-lfs) |
| [`aws`](https://docs.aws.amazon.com/cli/index.html) | Latest at build time. | Interface to Amazon Web Services. Dependency of S3 deployments. | [Python Package: `awscli`](https://pypi.org/project/awscli/) |
| GNU Compiler Collection                             | `9.3.x`               | C (`gcc`) and C++ (`g++`) compiler. Dependency of `npm install...` and therefore `shadow-cljs`. | [Ubuntu Package: `build-essential`](https://packages.ubuntu.com/focal/build-essential) |
| `make`                                              | Latest at build time. | Build automation tool, esp common for older C/C++ projects. Dependency of space-vim. | [Ubuntu Package: `build-essential`](https://packages.ubuntu.com/focal/build-essential) |
| `cmake`                                             | Latest at build time. | Build automation tool, esp common for newer C/C++ projects. Dependency of `gitstatusd`. |  [Ubuntu Package: `cmake`](https://packages.ubuntu.com/focal/cmake) |
| `zstd`                                              | Latest at build time. | Fast, lossless compression. Dependency of [`actions/cache@v2`](https://github.com/actions/cache). | [Ubuntu Package: `zstd`](https://packages.ubuntu.com/focal/zstd) |
| `Xvfb`                                              | Latest at build time. | X virtual framebuffer. Dependency of running Chrome without a display in GitHub Actions. | [Ubuntu Package: `xvfb`](https://packages.ubuntu.com/focal/xvfb) |
| `karma` CLI                                         | `2.0.0`               | Dependency of builds that use the [Karma test runner](https://karma-runner.github.io/latest/index.html). | [npm: `karma-cli`](https://www.npmjs.com/package/karma-cli) |
| Chromium                                            | `56.0.2924.0`         | Used as a browser to execute Karma tests. This specific old version tracks the version of [Electron](https://www.electronjs.org/) that Day8 has deployed. | [Long storey](https://github.com/day8/dockerfile-for-dev-ci-image/blob/5dd8bbc8032f9ed17fd75da378e4d03a3c00cd5b/Dockerfile#L334)  |
| ChromeDriver                                        | `2.29`                | Dependency of executing Karma tests in Chromium. | [Project Website](https://chromedriver.chromium.org/) |
| PhantomJS                                           | `2.1.1`               | Obsolete headless browser to execute Karma tests. As of 2020 still used by [`cljs-oss/canary`](https://github.com/cljs-oss/canary) builds. | [Bitbucket](https://bitbucket.org/ariya/phantomjs/downloads/) |

## REPLs

The following [REPLs](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) are
available for exploratory programming and debugging of code snippets.

| Name               | Language        | Example                                       |
| ------------------ | --------------- | --------------------------------------------- | 
| 'Official' Clojure | Clojure         | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 clojure`   |
| Leiningen          | Clojure         | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 lein repl` |
| Planck             | ClojureScript   | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 planck`    |
| Lumo               | ClojureScript   | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 lumo`      |
| Node.js            | JavaScript      | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 node`      |
| Python 2           | Python 2        | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 python2`   |
| Python 3           | Python 3        | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 python3`   |
| Bash               | Bash            | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 bash`      |
| PowerShell         | PowerShell Core | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0 pwsh`      |
| ZSH (default)      | ZSH             | `docker run -it --rm docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0`           |

## Command-Line Tools

The following tools are not usually required for builds (e.g. GitHub Actions). These are included for convenience and
usability when using the image as an interactive shell.

| Tool                              | Description | Origin |
| --------------------------------- | --- | --- |
| [`clj-kondo`](https://github.com/clj-kondo/clj-kondo) | A linter for Clojure code that sparks joy. | GitHub Releases Assets |
| [`babashka`](https://github.com/babashka/babashka) | Native Clojure interpreter for scripting. | GitHub Releases Assets |
| [`gh`](https://github.com/cli/cli/) | GitHub's official command line tool. | [GitHub Releases Assets: `gh_N.N.N_linux_amd64.deb`](https://github.com/cli/cli/releases) |
| [`exa`](https://the.exa.website/) | Modern replacement for `ls`. | [GitHub Releases Assets: `exa-linux-x86_64-N.N.N.zip`](https://github.com/ogham/exa/releases) |
| [`bat`](https://github.com/sharkdp/bat) | `cat` clone with syntax highlighting and Git integration. | [GitHub Releases Assets: `bat_N.N.N_amd64.deb`](https://github.com/sharkdp/bat/releases) |
| [`fd`](https://github.com/sharkdp/fd) | Simple, fast and user-friendly alternative to `find`. | [GitHub Releases Assets: `fd_N.N.N_amd64.deb`](https://github.com/sharkdp/fd/releases) |
| [`fzf`](https://github.com/junegunn/fzf) | Fuzzy finder. | [Ubuntu Package: `fzf`](https://packages.ubuntu.com/focal/fzf) |
| [`rgrep`](https://github.com/BurntSushi/ripgrep) ('ripgrep') | `grep` that respects `.gitignore` and automatically skips hidden files/directories and binary files. | [GitHub Releases Assets: `ripgrep_N.N.N_amd64.deb`](https://github.com/BurntSushi/ripgrep/releases) |
| [`ag`](https://github.com/ggreer/the_silver_searcher) | The silver searcher, a code-searching tool similar to `ack`. | [Ubuntu Package: `silversearcher-ag`](https://packages.ubuntu.com/focal/silversearcher-ag) |
| [`websocat`](https://github.com/vi/websocat) | Client for WebSockets, like `curl` for `ws://`. | [GitHub Releases Assets: `websocat_N.N.N_ssl1.1_amd64.deb`](https://github.com/vi/websocat/releases) |
| [`pueue`](https://github.com/Nukesor/pueue) | Task management for sequential and parallel execution of long-running tasks. | [GitHub Releases Assets: `pueue-linux-x86_64` and `pueued-linux-x86_64`](https://github.com/Nukesor/pueue/releases) |
| [`tmux`](https://github.com/tmux/tmux/wiki) | Terminal multiplexer. | [Ubuntu Package: `tmux`](https://packages.ubuntu.com/focal/tmux) |
| [`rlwrap`](https://github.com/hanslub42/rlwrap)              | A 'readline wrapper' to allow the editing of keyboard input for any command. | [Ubuntu Package: `rlwrap`](https://packages.ubuntu.com/focal/rlwrap) |
| [`diff-so-fancy`](https://github.com/so-fancy/diff-so-fancy) | Human readable diffs. | [npm: `diff-so-fancy`](https://www.npmjs.com/package/diff-so-fancy) |
| [`diffstat`](https://invisible-island.net/diffstat/) | Make a histogram of diffs. | [Ubuntu Package: `diffstat`](https://packages.ubuntu.com/focal/diffstat) |
| [`jq`](https://stedolan.github.io/jq/) | Like `sed` for JSON. | [Ubuntu Package: `jq`](https://packages.ubuntu.com/focal/jq) |
| [`yq`](https://kislyuk.github.io/yq/) | `jq` wrapper for YAML and XML. |  [Python Package: `yq`](https://pypi.org/project/yq/) |
| [`hexyl`](https://github.com/sharkdp/hexyl) | Hex viewer. | [GitHub Releases Assets: `hexyl_N.N.N_amd64.deb`](https://github.com/sharkdp/hexyl/releases) |
| [`neofetch`](https://github.com/dylanaraps/neofetch) | System information tool. | [Ubuntu Package: `neofetch`](https://packages.ubuntu.com/focal/neofetch) |
| [`htop`](https://hisham.hm/htop/) | Interactive process viewer. | [Ubuntu Package: `htop`](https://packages.ubuntu.com/focal/htop) |
| [`ncdu`](https://en.wikipedia.org/wiki/Ncdu) | Interactive disk usage analyzer. | [Ubuntu Package: `ncdu`](https://packages.ubuntu.com/focal/ncdu) |
| [`ssh`](https://www.openssh.com/) | OpenSSH client. | [Ubuntu Package: `openssh-client`](https://packages.ubuntu.com/focal/openssh-client) |
| [`mosh`](https://mosh.org/) | More robust and responsive SSH client. | [Ubuntu Package: `mosh`](https://packages.ubuntu.com/focal/mosh) |
| [`pngnq`](http://pngnq.sourceforge.net/) | Lossy PNG compressor. | [Ubuntu Package: `pngnq`](https://packages.ubuntu.com/focal/pngnq) | 
| [`pngquant`](https://pngquant.org/) | Lossy PNG compressor. | [Ubuntu Package: `pngquant`](https://packages.ubuntu.com/focal/pngquant) |
| [`pngcrush`](https://en.wikipedia.org/wiki/Pngcrush) | Lossless PNG compressor. | [Ubuntu Package: `pngcrush`](https://packages.ubuntu.com/focal/pngcrush) |
| `pngtools` | Series of tools for PNGs. | [Ubuntu Package: `pngtools`](https://packages.ubuntu.com/focal/pngtools) |
| [`pngmeta`](http://www.libpng.org/pub/png/apps/pngmeta.html) | Extracts metadata from PNGs. | [Ubuntu Package: `pngmeta`](https://packages.ubuntu.com/focal/pngmeta) |
| [`pngcheck`](http://www.libpng.org/pub/png/apps/pngcheck.html) | Verifies integrity of PNGs. | [Ubuntu Package: `pngcheck`](https://packages.ubuntu.com/focal/pngcheck) |
| [`jpegoptim`](https://github.com/tjko/jpegoptim) | Lossy JPEG compressor. | [Ubuntu Package: `jpegoptim`](https://packages.ubuntu.com/focal/jpegoptim) |
| [`jhead`](https://packages.ubuntu.com/focal/jhead) | JPEG metadata manipulation tool. | [Ubuntu Package: `jhead`](https://packages.ubuntu.com/focal/jhead) |
| `jpegpixi` | Removes defects from JPEGs. | [Ubuntu Package: `jpegpixi`](https://packages.ubuntu.com/focal/jpegpixi) |
| [`jpeginfo`](https://github.com/tjko/jpeginfo) | Verifies integrity of JPEGs. | [Ubuntu Package: `jpeginfo`](https://packages.ubuntu.com/focal/jpeginfo) |

## Editors

Usually editing of source files is done outside of the container and either synced with 
[Okteto]([Okteto](https://okteto.com/)) or checked out with Git in the case of GitHub Actions.

However, on rare occasions it may be useful to shell into the container to edit a file. To make such
situations less painful we include the following common editors:

| Name                                           | Configuration                                        | Description                              | Origin                                                                                     | 
| ---------------------------------------------- | ---------------------------------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------ |
| [`nano`](https://www.nano-editor.org/)         | Ubuntu Defaults                                      | The simplest editor. Good for beginners. | [Ubuntu Package: `nano`](https://packages.ubuntu.com/focal/nano)                           |
| [`nvim`](https://neovim.io/) ('NeoVim')        | [space-vim](https://github.com/liuchengxu/space-vim) | A better `vim`.                          | ['Neovim Unstable' Ubuntu PPA](https://launchpad.net/~neovim-ppa/+archive/ubuntu/unstable) |
| [`emacs`](https://www.gnu.org/software/emacs/) | [Spacemacs](https://www.spacemacs.org/)              | For the grey-beards.                     | [Ubuntu Package: `emacs-nox`](https://packages.ubuntu.com/focal/emacs-nox)                 |

## Why Not Docker Hub ?

In 2020 [Docker Hub introduced rate limiting](https://www.docker.com/blog/scaling-docker-to-serve-millions-more-developers-network-egress/)
and around the same time [GitHub Actions finally fixed use of GitHub Container Registry](https://github.blog/changelog/2020-09-24-github-actions-private-registry-support-for-job-and-service-containers/).

Therefore we no longer publish images to Docker Hub and use GitHub Container Registry instead.

## GitHub Actions Example

```yaml
jobs:
  test:
    name: Test
    runs-on: ubuntu-18.04
    container:
      image: docker.pkg.github.com/day8/dockerfile-for-dev-ci-image/dev-ci:0.1
      credentials:
        username: ${{ github.actor }}
        password: ${{ secrets.GLOBAL_TOKEN_FOR_GITHUB }} # <-- you need to create a GitHub Secret with a manual token that has global access as github.token only has access to the current repo! 
```

## Troubleshooting

### Upgrading Chrome / Chromium

#### Problem

The version of Chromium used needs to be upgraded to match an Electron upgrade.

#### Solution

Hold on to your hat and [walk this path](https://github.com/day8/dockerfile-for-dev-ci-image/blob/5dd8bbc8032f9ed17fd75da378e4d03a3c00cd5b/Dockerfile#L334).

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
`day8au/dev-ci:1.2.3`, `day8au/dev-ci:1.2` and `day8au/dev-ci:1`. E.g.:

```shell
$ git tag v1.2.3 HEAD
$ git push --tags
```

## License

This repository is [MIT licenced](license.txt)
