FROM ubuntu:20.04

ENV JVM_OPTS=-Xmx4g
ENV LEIN_ROOT=true
ENV DISPLAY=:99

SHELL ["/bin/bash", "-c"]

# Every RUN command creates an intermediate layer. Every additional layer has a massive performance
# impact on the 'Initialize containers' step in GitHub Actions, which in turn is by far the most
# significant step in duration in most GitHub Actions workflows. Therefore we execute and cleanup
# everything in a single RUN command to maximise performance.
RUN \
    # Just like RUN commands, every ENV line creates a new intermediate layer. So ENV lines are
    # reserved only for those environment variables that must persist after this RUN script has
    # completed.
    export JVM_VERSION="11" && \
    export LEIN_VERSION="2.9.6" && \
    export LEIN_INSTALL="/usr/local/bin" && \
    export LEIN_SHA256SUM="094b58e2b13b42156aaf7d443ed5f6665aee27529d9512f8d7282baa3cc01429" && \
    export LEIN_GPGKEY="20242BACBBE95ADA22D0AFD7808A33D379C806C3" && \
    export CLOJURE_VERSION="1.10.3.943" && \
    export CLOJURE_SHA256SUM="f1fdb786fa8b9ef3a08d0b331a51861cd5a6eea277e93bbad64bf37774df17c6" && \
    export BABASHKA_VERSION="0.5.1" && \
    export BABASHKA_SHA256SUM="bfd6e39f068fecfc9d0197daf78615b334cc93635056d92d329fd89ce2106b28" && \
    export CLJ_KONDO_VERSION="2021.08.06" && \
    export CLJ_KONDO_SHA256SUM="198bb53ee2bd8d9ccd317bc771474a966c4f8cf5890102c20b8770aa8a783c8c" && \
    export TERRAFORM_VERSION="1.0.4" && \
    export TERRAFORM_SHA256SUM="5c0be4d52de72143e2cd78e417ee2dd582ce229d73784fd19444445fa6e1335e" && \
    export AWS_CLI_VERSION="2.2.31" && \
    export AWS_CLI_SHA256SUM="4cf74631929e75acbb687b11f2d6644c7159eb95e76df22da4f4a4a74f92a355" && \
    export AWS_SAM_CLI_VERSION="1.29.0" && \
    export AWS_SAM_CLI_SHA256SUM="ed73fa3ac2245865f5eac8b902ef882f2bb4bde1b613311cf990641f32cdb483" && \
    export HASURA_CLI_VERSION="2.0.7" && \
    export HASURA_CLI_SHA256SUM="a314f39e2c9edcd2f113c005a80f2c69edc4700127663a5e7eb78d7f208f2acc" && \
    export LUMO_VERSION="1.10.1" && \
    export KARMA_CLI_VERSION="2.0.0" && \
    export GH_VERSION="2.0.0" && \
    export GH_SHA256SUM="7108b8405dc88ec83e351646b7804315e0be51714aa8c759c6b57221c8310eb2" && \
    export BAT_VERSION="0.18.3" && \
    export BAT_SHA256SUM="3e85d15e5757d9f56af8f15bb99a56aaaab33ed541fa3ff1e1c585f5733845c3" && \
    export DELTA_VERSION="0.8.3" && \
    export DELTA_SHA256SUM="b73f0e545f7ad1f4e55e0ef5b55efac26ce19c6b5a967ae7859ed38bd8508472" && \
    export FD_VERSION="8.2.1" && \
    export FD_SHA256SUM="f3a949325f1893145ced2b269a67d5763af3bede435c40e3b85b29afdb78c3d2" && \
    export HEXYL_VERSION="0.9.0" && \
    export HEXYL_SHA256SUM="9bf99a1441f2261fe3548a3b886e70a6e7bf78dc501b8209c2bbe342a535b194" && \
    export RIPGREP_VERSION="13.0.0" && \
    export RIPGREP_SHA256SUM="6d78bed13722019cb4f9d0cf366715e2dcd589f4cf91897efb28216a6bb319f1" && \
    export NEOVIM_VERSION="0.5.0" && \
    export NEOVIM_SHA256SUM="cd59e885143e0fa7e43cfdacbc8e37dc033c56d9205b8f8a8acd08cd9f93737d" && \
    export EXA_VERSION="0.10.1" && \
    export EXA_SHA256SUM="a65a87bd545e969979ae9388f6333167f041a1f09fa9d60b32fd3072348ff6ce" && \
    export WEBSOCAT_VERSION="1.8.0" && \
    export WEBSOCAT_SHA256SUM="5968a3a2ede69fa8bb3df1419b908575f6532ce4eef250e1fcfbaadada3ccaef" && \
    export PUEUE_VERSION="1.0.1" && \
    export PUEUE_SHA256SUM="8e7cc049b4ebc475e7fe8336f8601efc529632b6468da417bea43870ec4c9d94" && \
    export PUEUED_SHA256SUM="b87573819b57a97cca62c68c7f9af5cc713d06ab019409dd676a8953fc376983" && \
    export POWERSHELL_VERSION="7.1.4" && \
    export POWERSHELL_SHA256SUM="7435b5cdd8cbedeee396072b495b44067166674e7c40a9ee844a519c6223e482" && \
    export OH_MY_POSH_VERSION="3.175.0" && \
    export OH_MY_POSH_SHA256SUM="ccb67953ecb545c7b8aa1b56dfb40844c1e7dec4b7cb6ee3695da36cd3042396" && \
    export DOCKER_VERSION="20.10.8" && \
    export DOCKER_SHA256SUM="7ea11ecb100fdc085dbfd9ab1ff380e7f99733c890ed815510a5952e5d6dd7e0" && \
    export DOCKER_COMPOSE_VERSION="1.29.2" && \
    export DOCKER_COMPOSE_SHA256SUM="f3f10cf3dbb8107e9ba2ea5f23c1d2159ff7321d16f0a23051d68d8e2547b323" && \
    export KUBECTL_VERSION="1.22.0" && \
    export KUBECTL_SHA256SUM="703e70d49b82271535bc66bc7bd469a58c11d47f188889bd37101c9772f14fa1" && \
    export GIT_GPGKEY="E1DD270288B4E6030699E45FA1715D88E1DF1F24" && \
    export PLANCK_GPGKEY="A5D6812987A6E53579AF0308D3D743111F327606" && \
    export PHANTOMJS_VERSION="2.1.1" && \
    export PHANTOMJS_SHA256SUM="86dd9a4bf4aee45f1a84c9f61cf1947c1d6dce9b9e8d2a907105da7852460d2f" && \
    export DEBIAN_FRONTEND="noninteractive" && \
    export DOTFILES_BASE_URI="https://raw.githubusercontent.com/day8/dockerfile-for-dev-ci-image/master/dotfiles/" && \

    cd /tmp && \

    # Turn on Bash extended glob support so we can use patterns like !("file1"|"file2")
    shopt -s extglob && \

    apt-get update -qq && \
    apt-get dist-upgrade -qq -y && \
    echo '\n\n' && \

    # Install tools needed by the subsequent commands.
    apt-get install -qq -y --no-install-recommends \
        locales tzdata \
        ca-certificates gnupg \
        curl wget \
        unzip && \
    echo '\n\n' && \

    # Set the default locale to en_AU.UTF-8
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# en_AU.UTF-8 UTF-8/en_AU.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen && \
    echo 'LANG="en_AU.UTF-8"' > /etc/default/locale && \
    dpkg-reconfigure locales && \
    update-locale LANG=en_AU.UTF-8 && \

    # Set the default timezone to Australia/Sydney.
    ln -s --force /usr/share/zoneinfo/Australia/Sydney /etc/localtime && \
    dpkg-reconfigure tzdata && \

    # When adding PPAs do not depend on `software-properties-common` for the `add-apt-repository`
    # command as that adds several hundred megabytes of dependencies.
    echo "Adding 'Git stable releases' PPA..." && \
    echo "deb http://ppa.launchpad.net/git-core/ppa/ubuntu focal main" > /etc/apt/sources.list.d/git-core-ubuntu-ppa-focal.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $GIT_GPGKEY && \
    echo '\n\n' && \

    echo "Adding 'Mike Fikes's Planck' PPA..." && \
    echo "deb http://ppa.launchpad.net/mfikes/planck/ubuntu focal main" > /etc/apt/sources.list.d/mfikes-ubuntu-planck-focal.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $PLANCK_GPGKEY && \
    echo '\n\n' && \

    # Git Large File Storage is required when cloning any Git repository that uses the Git LFS
    # Specification. See https://github.com/git-lfs/git-lfs
    echo "Adding Git LFS package repository..." && \
    curl -sSL https://packagecloud.io/github/git-lfs/gpgkey | apt-key add - && \
    echo "deb https://packagecloud.io/github/git-lfs/ubuntu/ focal main" > /etc/apt/sources.list.d/github_git-lfs.list && \
    echo '\n\n' && \

    echo "Adding NodeSource's Node.js v14.x package repository..." && \
    echo "deb https://deb.nodesource.com/node_14.x focal main" > /etc/apt/sources.list.d/nodesource.list && \
    curl -sS https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
    echo '\n\n' && \

    echo "Adding Yarn package repository..." && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo '\n\n' && \

    # Refresh package lists after adding package repositories.
    apt-get update -qq && \

    # Begin /docker-entrypoint.sh script. This is amended throughout the RUN
    # command with additional lines.
    echo '#!/bin/bash' > /docker-entrypoint.sh && \

    # Install development tooling, common editors and language runtimes.
    apt-get install -qq -y --no-install-recommends \
      git git-lfs diffstat jq silversearcher-ag fzf \
      neofetch ncdu htop zstd \
      rlwrap less tmux openssh-client mosh \
      neovim emacs-nox \
      zsh build-essential cmake openjdk-$JVM_VERSION\-jdk-headless nodejs yarn python2 python3-pip planck \
      pngnq pngquant pngtools pngmeta pngcrush pngcheck \
      jhead jpeginfo jpegoptim jpegpixi \
    # These are dependencies of powershell
      liblttng-ust-ctl4 liblttng-ust0 \
    # These are dependencies of xvfb, Chrome and ChromeDriver that are common to all versions.
      xvfb libxcomposite1 libxrandr2 libxss1 \
    # Voting in the popularity contest may help keep packages we use supported by Ubuntu.
      popularity-contest && \
    rm -rf /var/lib/apt/lists/* && \
    echo '\n\n' && \

    # Start a X11 virtual framebuffer to run tests in Chrome.
    echo 'Xvfb :99 -screen 0 1920x1080x24 &\n' >> /docker-entrypoint.sh && \

    # Install Clojure:
    echo "Installing 'Official' Clojure CLI ${CLOJURE_VERSION}..." && \
    wget -q "https://download.clojure.org/install/linux-install-$CLOJURE_VERSION.sh" && \
    echo "Verifying linux-install-$CLOJURE_VERSION.sh checksum..." && \
    sha256sum linux-install-$CLOJURE_VERSION.sh && \
    echo "$CLOJURE_SHA256SUM *linux-install-$CLOJURE_VERSION.sh" | sha256sum -c - && \
    chmod +x linux-install-$CLOJURE_VERSION.sh && \
    ./linux-install-$CLOJURE_VERSION.sh && \
    rm -f "linux-install-${CLOJURE_VERSION}.sh" && \
    clojure -e "(clojure-version)" && \
    echo '\n\n' && \

    # Install Leiningen:
    echo "Installing Leiningen ${LEIN_VERSION}..." && \
    mkdir -p $LEIN_INSTALL && \
    wget -q https://raw.githubusercontent.com/technomancy/leiningen/$LEIN_VERSION/bin/lein-pkg && \
    echo "Verifying lein-pkg checksum..." && \
    sha256sum lein-pkg && \
    echo "$LEIN_SHA256SUM *lein-pkg" | sha256sum -c - && \
    mv lein-pkg $LEIN_INSTALL/lein && \
    chmod 0755 $LEIN_INSTALL/lein && \
    wget -q "https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip" && \
    wget -q "https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip.asc" && \
    gpg --batch --keyserver keys.openpgp.org --recv-key $LEIN_GPGKEY && \
    echo "Verifying leiningen-$LEIN_VERSION-standalone.zip PGP signature..." && \
    gpg --batch --verify leiningen-$LEIN_VERSION-standalone.zip.asc leiningen-$LEIN_VERSION-standalone.zip && \
    rm leiningen-$LEIN_VERSION-standalone.zip.asc && \
    mkdir -p /usr/share/java && \
    mv leiningen-$LEIN_VERSION-standalone.zip /usr/share/java/leiningen-$LEIN_VERSION-standalone.jar && \
    mkdir -p /root/.lein && \
    wget -q -O /root/.lein/profiles.clj "${DOTFILES_BASE_URI}/.lein/profiles.clj" && \
    echo '\n\n' && \

    # Install babashka:
    echo "Installing babashka..." && \
    wget -q "https://github.com/babashka/babashka/releases/download/v${BABASHKA_VERSION}/babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" && \
    echo "Verifying babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz checksum..." && \
    sha256sum "babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" && \
    echo "${BABASHKA_SHA256SUM} *babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" | sha256sum -c - && \
    tar -xzf "babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" -C /usr/local/bin && \
    bb --version && \
    
    # Install clj-kondo:
    echo "Installing clj-kondo..." && \
    wget -q "https://github.com/clj-kondo/clj-kondo/releases/download/v${CLJ_KONDO_VERSION}/clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" && \
    echo "Verifying clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip checksum..." && \
    sha256sum "clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" && \
    echo "$CLJ_KONDO_SHA256SUM *clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" | sha256sum -c - && \
    unzip "clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" -d /usr/local/bin && \
    clj-kondo --version && \
    
    # Install Terraform:
    echo "Installing Terraform..." && \
    wget -q "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    echo "Verifying terraform_${TERRAFORM_VERSION}_linux_amd64.zip checksum..." && \
    sha256sum "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    echo "${TERRAFORM_SHA256SUM} *terraform_${TERRAFORM_VERSION}_linux_amd64.zip" | sha256sum -c - && \
    unzip "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" -d /usr/local/bin && \
    terraform --version && \

    # install AWS CLI
    echo "Install AWS CLI..." && \
    wget -q "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip" && \
    echo "Verifying awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip checksum..." && \
    sha256sum awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip && \
    echo "${AWS_CLI_SHA256SUM} awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip" | sha256sum -c - && \
    unzip awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip -d aws-installation && \
    ./aws-installation/aws/install && \
    rm -rf awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip aws-installation && \
    aws --version && \

    # Install AWS SAM CLI
    echo "Install AWS SAM CLI..." && \
    wget -q "https://github.com/aws/aws-sam-cli/releases/download/v${AWS_SAM_CLI_VERSION}/aws-sam-cli-linux-x86_64.zip" && \
    echo "Verifying aws-sam-cli-linux-x86_64.zip checksum.." && \
    sha256sum aws-sam-cli-linux-x86_64.zip && \
    echo "${AWS_SAM_CLI_SHA256SUM} aws-sam-cli-linux-x86_64.zip" | sha256sum -c - && \
    unzip aws-sam-cli-linux-x86_64.zip -d sam-installation && \
    ./sam-installation/install && \
    rm -rf aws-sam-cli-linux-x86_64.zip sam-installation && \
    sam --version && \
    
    ## Install Hasura CLI
    echo "Install Hasura CLI..." && \
    wget -q "https://github.com/hasura/graphql-engine/releases/download/v${HASURA_CLI_VERSION}/cli-hasura-linux-amd64" && \
    echo "Verifying cli-hasura-linux-amd64 checksum..." && \
    sha256sum cli-hasura-linux-amd64 && \
    echo "${HASURA_CLI_SHA256SUM} *cli-hasura-linux-amd64" | sha256sum -c - && \
    mv cli-hasura-linux-amd64 /usr/local/bin/hasura && \
    chmod +x /usr/local/bin/hasura && \
    hasura version && \
    
    # Install pipenv
    echo "Installing pipenv..." && \
    pip3 -q install pipenv && \
    echo '\n\n' && \

    # Install yq (jq wrapper for YAML and XML)
    echo "Installing yq..." && \
    pip3 -q install yq && \
    echo '\n\n' && \

    # Install pytest
    echo "Installing pytest..." && \
    pip3 -q install pytest && \
    echo '\n\n' && \

    # Install flake8
    # Ref: https://gitlab.com/pycqa/flake8
    echo "Installing flake8..." && \
    pip3 -q install flake8 && \
    echo '\n\n' && \

    # Install Docker
    # Origin: https://download.docker.com/linux/static/stable/x86_64/
    echo "Installing Docker ${DOCKER_VERSION}..." && \
    wget -q "https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz" && \
    echo "Verifying docker-${DOCKER_VERSION}.tgz checksum..." && \
    sha256sum "docker-${DOCKER_VERSION}.tgz" && \
    echo "${DOCKER_SHA256SUM} *docker-${DOCKER_VERSION}.tgz" | sha256sum -c - && \
    tar -xzC /usr/local/bin --strip-components=1 -f "docker-${DOCKER_VERSION}.tgz" && \
    rm -f "docker-${DOCKER_VERSION}.tgz" && \

    # Install Docker Compose
    echo "Installing Docker Compose ${DOCKER_COMPOSE_VERSION}..." && \
    wget -q "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-Linux-x86_64" && \
    echo "Verifying docker-compose-Linux-x86_64 checksum..." && \
    sha256sum "docker-compose-Linux-x86_64" && \
    echo "${DOCKER_COMPOSE_SHA256SUM} *docker-compose-Linux-x86_64" | sha256sum -c - && \
    mv "docker-compose-Linux-x86_64" /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose && \

    # Install Kubectl
    echo "Installing Kubectl ${KUBECTL_VERSION}..." && \
    wget -q "https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl" && \
    echo "Verifying Kubectl checksum..." && \
    sha256sum kubectl && \
    echo "${KUBECTL_SHA256SUM} *kubectl" | sha256sum -c - && \
    mv kubectl /usr/local/bin && \
    chmod +x /usr/local/bin/kubectl && \

    # Install gh
    #
    # GitHub's official command line tool.
    #
    # To update to a new release:
    # 1. Open https://github.com/cli/cli/releases/
    # 2. Download gh_VERSION_linux_amd64.deb for the desired version
    # 3. Run `sha256sum gh_VERSION_linux_amd64.deb`
    # 4. Copy/paste the checksum to GH_SHA256SUM in environment variables at the beginning of this RUN script
    # 5. Edit the GH_VERSION in environment variables at the beginning of this RUN script
    echo "Installing gh ${GH_VERSION}..." && \
    wget -q "https://github.com/cli/cli/releases/download/v${GH_VERSION}/gh_${GH_VERSION}_linux_amd64.deb" && \
    echo "Verifying gh_${GH_VERSION}_linux_amd64.deb checksum..." && \
    sha256sum "gh_${GH_VERSION}_linux_amd64.deb" && \
    echo "$GH_SHA256SUM *gh_${GH_VERSION}_linux_amd64.deb" | sha256sum -c - && \
    dpkg -i "gh_${GH_VERSION}_linux_amd64.deb" && \
    rm -f "gh_${GH_VERSION}_linux_amd64.deb" && \
    echo '\n\n' && \

    # Install bat
    #
    # A cat(1) clone with syntax highlighting and Git integration.
    #
    # To update to a new release:
    # 1. Open https://github.com/sharkdp/bat/releases
    # 2. Download bat_VERSION_amd64.deb for the desired version
    # 3. Run `sha256sum bat_VERSION_amd64.deb`
    # 4. Copy/paste the checksum to BAT_SHA256SUM in environment variables at the beginning of this RUN script
    # 5. Edit the BAT_VERSION in environment variables at the beginning of this RUN script
    echo "Installing bat ${BAT_VERSION}..." && \
    wget -q "https://github.com/sharkdp/bat/releases/download/v${BAT_VERSION}/bat_${BAT_VERSION}_amd64.deb" && \
    echo "Verifying bat_${BAT_VERSION}_amd64.deb checksum..." && \
    sha256sum "bat_${BAT_VERSION}_amd64.deb" && \
    echo "$BAT_SHA256SUM *bat_${BAT_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "bat_${BAT_VERSION}_amd64.deb" && \
    rm -f "bat_${BAT_VERSION}_amd64.deb" && \
    echo '\n\n' && \

    # Install delta
    #
    # A viewer for git and diff output.
    
    echo "Installing delta ${DELTA_VERSION}..." && \
    wget -q "https://github.com/dandavison/delta/releases/download/${DELTA_VERSION}/git-delta_${DELTA_VERSION}_amd64.deb" && \
    echo "Verifying git-delta_${DELTA_VERSION}_amd64.deb checksum..." && \
    sha256sum "git-delta_${DELTA_VERSION}_amd64.deb" && \
    echo "$DELTA_SHA256SUM *git-delta_${DELTA_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "git-delta_${DELTA_VERSION}_amd64.deb" && \
    rm -f "git-delta_${DELTA_VERSION}_amd64.deb" && \
    echo '\n\n' && \

    # Install fd
    #
    # fd is a simple, fast and user-friendly alternative to 'find'.
    #
    # To update to a new release:
    # 1. Open https://github.com/sharkdp/fd/releases
    # 2. Download fd_VERSION_amd64.deb for the desired version
    # 3. Run `sha256sum fd_VERSION_amd64.deb`
    # 4. Copy/paste the checksum to FD_SHA256SUM in environment variables at the beginning of this RUN script
    # 5. Edit the FD_VERSION in environment variables at the beginning of this RUN script
    echo "Installing fd ${FD_VERSION}..." && \
    wget -q "https://github.com/sharkdp/fd/releases/download/v${FD_VERSION}/fd_${FD_VERSION}_amd64.deb" && \
    echo "Verifying fd_${FD_VERSION}_amd64.deb checksum..." && \
    sha256sum "fd_${FD_VERSION}_amd64.deb" && \
    echo "$FD_SHA256SUM *fd_${FD_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "fd_${FD_VERSION}_amd64.deb" && \
    rm -f "fd_${FD_VERSION}_amd64.deb" && \
    echo '\n\n' && \

    # Install hexyl
    #
    # hexyl is a command-line hex viewer.
    #
    # To update to a new release:
    # 1. Open https://github.com/sharkdp/hexyl/releases
    # 2. Download hexyl_VERSION_amd64.deb for the desired version
    # 3. Run `sha256sum hexyl_VERSION_amd64.deb`
    # 4. Copy/paste the checksum to HEXYL_SHA256SUM in environment variables at the beginning of this RUN script
    # 5. Edit the HEXYL_VERSION in environment variables at the beginning of this RUN script
    echo "Installing hexyl ${HEXYL_VERSION}..." && \
    wget -q "https://github.com/sharkdp/hexyl/releases/download/v${HEXYL_VERSION}/hexyl_${HEXYL_VERSION}_amd64.deb" && \
    echo "Verifying hexyl package checksum..." && \
    sha256sum "hexyl_${HEXYL_VERSION}_amd64.deb" && \
    echo "$HEXYL_SHA256SUM *hexyl_${HEXYL_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "hexyl_${HEXYL_VERSION}_amd64.deb" && \
    rm -f "hexyl_${HEXYL_VERSION}_amd64.deb" && \
    echo '\n\n' && \

    # Install ripgrep
    echo "Installing ripgrep ${RIPGREP_VERSION}..." && \
    wget -q "https://github.com/BurntSushi/ripgrep/releases/download/${RIPGREP_VERSION}/ripgrep_${RIPGREP_VERSION}_amd64.deb" && \
    echo "Verifying ripgrep_${RIPGREP_VERSION}_amd64.deb checksum..." && \
    sha256sum "ripgrep_${RIPGREP_VERSION}_amd64.deb" && \
    echo "${RIPGREP_SHA256SUM} *ripgrep_${RIPGREP_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "ripgrep_${RIPGREP_VERSION}_amd64.deb" && \
    rm -f "ripgrep_${RIPGREP_VERSION}_amd64.deb" && \
    echo '\n\n' && \

    # Install exa
    echo "Installing exa ${EXA_VERSION}..." && \
    wget -q "https://github.com/ogham/exa/releases/download/v${EXA_VERSION}/exa-linux-x86_64-v${EXA_VERSION}.zip" && \
    echo "Verifying exa-linux-x86_64-v${EXA_VERSION}.zip checksum..." && \
    sha256sum "exa-linux-x86_64-v${EXA_VERSION}.zip" && \
    echo "${EXA_SHA256SUM} exa-linux-x86_64-v${EXA_VERSION}.zip" | sha256sum -c - && \
    unzip -q "exa-linux-x86_64-v${EXA_VERSION}.zip" -d /usr/local && \
    rm -f "exa-linux-x86_64-v${EXA_VERSION}.zip" && \
    echo '\n\n' && \

    # Install nvim
    echo "Installing Neovim ${NEOVIM_VERSION}..." && \
    wget -q "https://github.com/neovim/neovim/releases/download/v${NEOVIM_VERSION}/nvim-linux64.tar.gz" && \
    echo "Verifying nvim-linux64.tar.gz checksum..." && \
    sha256sum "nvim-linux64.tar.gz" && \
    echo "${NEOVIM_SHA256SUM} nvim-linux64.tar.gz" | sha256sum -c - && \
    tar --strip-components=1 -C /usr/local -xf nvim-linux64.tar.gz && \
    rm -f nvim-linux64.tar.gz && \
    echo '\n\n' && \

    # Install websocat
    echo "Installing websocat ${WEBSOCAT_VERSION}..." && \
    wget -q "https://github.com/vi/websocat/releases/download/v${WEBSOCAT_VERSION}/websocat_${WEBSOCAT_VERSION}_newer_amd64.deb" && \
    echo "Verifying websocat_${WEBSOCAT_VERSION}_newer_amd64.deb checksum..." && \
    sha256sum "websocat_${WEBSOCAT_VERSION}_newer_amd64.deb" && \
    echo "${WEBSOCAT_SHA256SUM} *websocat_${WEBSOCAT_VERSION}_newer_amd64.deb" | sha256sum -c - && \
    dpkg -i "websocat_${WEBSOCAT_VERSION}_newer_amd64.deb" && \
    rm -f "websocat_${WEBSOCAT_VERSION}_newer_amd64.deb" && \
    echo '\n\n' && \

    # Install pueue
    echo "Installing pueue and pueued ${PUEUE_VERSION}..." && \
    wget -q "https://github.com/Nukesor/pueue/releases/download/v${PUEUE_VERSION}/pueue-linux-x86_64" && \
    wget -q "https://github.com/Nukesor/pueue/releases/download/v${PUEUE_VERSION}/pueued-linux-x86_64" && \
    echo "Verifying pueue-linux-x86_64 checksum..." && \
    sha256sum pueue-linux-x86_64 && \
    echo "${PUEUE_SHA256SUM} *pueue-linux-x86_64" | sha256sum -c - && \
    echo "Verifying pueued-linux-x86_64 checksum..." && \
    sha256sum pueued-linux-x86_64 && \
    echo "${PUEUED_SHA256SUM} *pueued-linux-x86_64" | sha256sum -c - && \
    mv pueue-linux-x86_64 /usr/local/bin/pueue && \
    mv pueued-linux-x86_64 /usr/local/bin/pueued && \
    chmod +x /usr/local/bin/pueue /usr/local/bin/pueued && \
    echo '\n\n' && \

    # Install PhantomJS
    #
    # PhantomJS is a discontinued headless browser used for automated web page interaction, such as testing. Although
    # normal unit test builds can use Chrome via Karma, unfortunately PhantomJS is required by cljs-oss/canary builds.
    echo "Installing PhantomJS ${PHANTOMJS_VERSION}..." && \
    wget -q -O phantomjs.tar.bz2 https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-${PHANTOMJS_VERSION}-linux-x86_64.tar.bz2 && \
    echo "Verifying PhantomJS package checksum..." && \
    sha256sum phantomjs.tar.bz2 && \
    echo "$PHANTOMJS_SHA256SUM *phantomjs.tar.bz2" | sha256sum -c - && \
    mkdir -p /opt/phantomjs/${PHANTOMJS_VERSION} && \
    tar jxf phantomjs.tar.bz2 --strip-components=1 -C /opt/phantomjs/${PHANTOMJS_VERSION} && \
    rm -f phantomjs.tar.bz2 && \
    ln -s /opt/phantomjs/${PHANTOMJS_VERSION} /opt/phantomjs/latest && \
    ln -s /opt/phantomjs/latest/bin/phantomjs /usr/local/bin/phantomjs && \
    echo '\n\n' && \

    # Install PowerShell
    #
    # Windows 10 has become a surprisingly good platform for developers in recent times. Just like its important to have
    # a preferred editor handy, be it vim or emacs, it is also important to have a familiar shell available to those who
    # want to use it.
    echo "Installing PowerShell ${POWERSHELL_VERSION}..." && \
    wget -q https://github.com/PowerShell/PowerShell/releases/download/v${POWERSHELL_VERSION}/powershell_${POWERSHELL_VERSION}-1.ubuntu.20.04_amd64.deb && \
    echo "Verifying powershell package checksum..." && \
    sha256sum "powershell_${POWERSHELL_VERSION}-1.ubuntu.20.04_amd64.deb" && \
    echo "$POWERSHELL_SHA256SUM *powershell_${POWERSHELL_VERSION}-1.ubuntu.20.04_amd64.deb" | sha256sum -c - && \
    dpkg -i "powershell_${POWERSHELL_VERSION}-1.ubuntu.20.04_amd64.deb" && \
    rm -f "powershell_${POWERSHELL_VERSION}-1.ubuntu.20.04_amd64.deb" && \

    # Ref: https://ohmyposh.dev/docs/linux
    wget -q "https://github.com/JanDeDobbeleer/oh-my-posh/releases/download/v${OH_MY_POSH_VERSION}/posh-linux-amd64" && \
    echo "Verifying posh-linux-amd64 checksum..." && \
    sha256sum posh-linux-amd64 && \
    echo "${OH_MY_POSH_SHA256SUM} posh-linux-amd64" | sha256sum -c - && \
    mv posh-linux-amd64 /usr/local/bin/oh-my-posh && \
    chmod +x /usr/local/bin/oh-my-posh && \
    mkdir ~/.poshthemes && \
    wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip && \
    unzip ~/.poshthemes/themes.zip -d ~/.poshthemes && \
    chmod u+rw ~/.poshthemes/*.json && \
    rm ~/.poshthemes/themes.zip && \
    cp ~/.poshthemes/jandedobbeleer.omp.json ~ && \
    pwsh -Command "Set-PSRepository PSGallery -InstallationPolicy Trusted" && \
    pwsh -Command "Install-Module posh-git -Scope CurrentUser" && \
    pwsh -Command "Install-Module -Name PSReadLine -AllowPrerelease -Scope CurrentUser -Force -SkipPublisherCheck" && \
    pwsh -Command 'if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }' && \
    pwsh -Command 'echo "$env:POSH_GIT_ENABLED = $true" > $PROFILE' && \
    pwsh -Command 'echo "Import-Module PSReadLine" > $PROFILE' && \
    pwsh -Command 'echo "oh-my-posh --init --shell pwsh --config ~/jandedobbeleer.omp.json | Invoke-Expression" > $PROFILE' && \
    echo '\n\n' && \

    # gitstatus is a 10x faster alternative to `git status` and `git describe`.
    #
    # Included in the Docker image so that the shell won't auto download it on
    # startup, reducing startup time.
    git clone --depth=1 https://github.com/romkatv/gitstatus.git && \
    cd gitstatus && \
    ./build -w && \
    mv usrbin/gitstatusd /usr/local/bin/gitstatusd && \
    cd /tmp && \
    rm -rf gitstatus && \

    # Install Zsh, Bash and Vim configuration files:
    echo 'eval "$(oh-my-posh --init --shell zsh --config ~/jandedobbeleer.omp.json)"' >> ~/.zshrc && \
    
    echo 'eval "$(oh-my-posh --init --shell bash --config ~/jandedobbeleer.omp.json)"' >> ~/.bashrc && \

    git clone --depth 1 https://github.com/liuchengxu/space-vim.git ~/.space-vim && \
    wget -q -O /root/.spacevim "${DOTFILES_BASE_URI}/.spacevim" && \
    cd ~/.space-vim && make neovim && cd /tmp && \

    wget -q -O /root/.gitconfig "${DOTFILES_BASE_URI}/.gitconfig" && \

    # Install NPM-based tools incl Lumo, Karma and Diff so Fancy:
    echo "Installing Lumo, Karma CLI and Diff so Fancy..." && \
    npm install -g --unsafe-perm \
      lumo-cljs@$LUMO_VERSION \
      karma-cli@$KARMA_CLI_VERSION \
      diff-so-fancy@$DIFF_SO_FANCY_VERSION && \

    # Finish Docker ENTRYPOINT script:
    echo 'neofetch' >> /docker-entrypoint.sh && \
    echo "echo \"`bb --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`lein version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"lumo `lumo --version` and Planck `planck --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"Node.js `node --version` with NPM `npm --version` and Yarn `yarn --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`python2 --version` and `python3 --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`aws --version` and `sam --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`git --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`git-lfs --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`pwsh --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"PhantomJS `phantomjs --version`\"" >> /docker-entrypoint.sh && \
    echo 'exec "$@"' >> /docker-entrypoint.sh && \
    chmod +x /docker-entrypoint.sh && \

    # GitHub Actions overrides ENTRYPOINT with the --entrypoint CLI option to execute a long-running
    # tail command so that once created the container does not exit until explicitly stopped.
    #
    # So on GitHub Actions docker-entrypoint.sh is never executed. CircleCI allowed the use of a metadata
    # LABEL com.circleci.preserve-entrypoint=true, but unfortunately GitHub Actions does not appear
    # to have any mechanism to preserve the entrypoint.
    #
    # To exacerbate the confusion newer versions of ChromeDriver such as used in the CircleCI images,
    # which are tied to newer version of Chrome, do not appear to require Xvfb to be running. The
    # version of ChromeDriver that we are using currently does require Xvfb to be started beforehand.
    #
    # To workaround this issue so that we don't need to start Xvfb manually in every GitHub Action
    # workflow YAML we replace /usr/bin/tail with our own script that first executes
    # docker-entrypoint.sh (and thus Xvfb) before executing a backed up copy of tail at /usr/bin/tail.original
    #
    mv /usr/bin/tail /usr/bin/tail.original && \
    echo '#!/bin/sh' > /usr/bin/tail && \
    echo 'exec /docker-entrypoint.sh /usr/bin/tail.original $@' >> /usr/bin/tail && \
    chmod +x /usr/bin/tail && \

    # Strip binaries of debugging symbols that are not stripped already so save
    # some space:
    strip --strip-unneeded \
        /usr/bin/git-lfs \
        /usr/local/bin/docker \
        /usr/local/bin/dockerd \
        /usr/local/bin/docker-proxy && \

    # Save about ~22MB by deleting the pip cache:
    rm -rf /root/.cache && \

    # Delete npm's cache:
    rm -rf /root/.npm && \

    # Delete the .m2 repository because we cache this with actions/cache in GitHub Actions:
    rm -rf /root/.m2 && \

    # Cleanup
    rm -rf /usr/share/icons/* \
           /var/lib/apt/lists/*

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/bin/bash"]
