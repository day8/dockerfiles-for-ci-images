## N.N.N (YYYY-MM-DD)

> Committed but unreleased changes are put here, at the top. Older releases are detailed chronologically below. 

## Unreleased

### Changed

- Upgrade AWS CLI to 2.4.15

### Removed

- Remove PhantomJS
- Remove gitstatusd

## 2.0.0 (2021-11-06)

#### Changed

- Migrated from docker.pkg.github.com to ghcr.io
- Split image into `day8/core`, `day8/chrome-56` and `day8/chrome-latest`
- Upgraded many of the included software packages.

## 1.0.0 (2021-06-21)

#### Added

- Add oh-my-posh v3 for all shells (PowerShell, ZSH, Bash).
- Add AWS SAM CLI. Dependency of [holy lambda](https://github.com/FieryCod/holy-lambda)

#### Changed

- Upgrade babashka to 0.4.6. Dependency of [holy lambda](https://github.com/FieryCod/holy-lambda)
- Upgrade clj-kondo to 2021.06.18. Dependency of [holy lambda](https://github.com/FieryCod/holy-lambda)
- Upgrade AWS CLI to 2.2.13 and use official installation instead of pip. Dependency of [holy lambda](https://github.com/FieryCod/holy-lambda)
- Upgrade Clojure CLI to 1.10.3.855
- Upgrade leiningen to 2.9.6
- Upgrade GitHub CLI to 1.11.0
- Upgrade bat to 0.18.1
- Upgrade ripgrep to 13.0.0
- Upgrade exa to 0.10.1
- Upgrade websocat to 1.8.0
- Upgrade pueue to 1.0.0-rc.2
- Upgrade PowerShell to 7.1.3
- Upgrade Docker to 20.10.7
- Upgrade docker-compose to 1.29.2
- Upgrade kubectl to 1.21.2

#### Removed

- Remove boot.
- Shell configuration frameworks other than oh-my-posh.


## 0.1.0 (2021-03-09)

Initial release.
