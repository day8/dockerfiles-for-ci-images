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
    export NODEJS_VERSION="16.18.0" && \
    export NODEJS_SHA256SUM="a50dd97f8deb363c61d7026e5f0abc0f140916d7fcabcc549e9444c1f5c97f03" && \
    export LEIN_VERSION="2.9.10" && \
    export LEIN_INSTALL="/usr/local/bin" && \
    export LEIN_SHA256SUM="dbb84d13d6df5b85bbf7f89a39daeed103133c24a4686d037fe6bd65e38e7f32" && \
    export LEIN_STANDALONE_JAR="895a0a0d-f20d-4580-a277-e06b5eec3b6b" && \
    export CLOJURE_VERSION="1.11.1.1413" && \
    export CLOJURE_SHA256SUM="ad9aa1e99c59a4f7eb66450914fbec543337d9fada60dd9d34eec7fe18ae4965" && \
    export BABASHKA_VERSION="1.0.164" && \
    export BABASHKA_SHA256SUM="b0d27d2c0080d60974fad3106e913718ed0b50922c2c33934b34c108975e0ea2" && \
    export CLJ_KONDO_VERSION="2022.10.14" && \
    export CLJ_KONDO_SHA256SUM="1fead3bd0763f83357fb0d5b7a5b9590ec6e10522c5d9176c5a405412142b907" && \
    export TERRAFORM_VERSION="1.0.10" && \
    export TERRAFORM_SHA256SUM="a221682fcc9cbd7fde22f305ead99b3ad49d8303f152e118edda086a2807716d" && \
    export AWS_CLI_VERSION="2.4.15" && \
    export AWS_CLI_SHA256SUM="9c243c0f83b736eaf1cb37fcc02a927799c771533531c5be67ba19778d56a43b" && \
    export AWS_SAM_CLI_VERSION="1.33.0" && \
    export AWS_SAM_CLI_SHA256SUM="3fcdf752ab30e6355087bcfa451a7d3ca6eb23445b893ecfcd0dea4fab166382" && \
    export HASURA_CLI_VERSION="2.13.0" && \
    export HASURA_CLI_SHA256SUM="aea853476164e70dfd7b12cc85933bc0accb6747e474420cb3c6e9af005be4d4" && \
    export KARMA_CLI_VERSION="2.0.0" && \
    export GH_VERSION="2.18.1" && \
    export GH_SHA256SUM="aca6852c457ae975155c936ad5c2691ff65a62a33142c94722bf76034a7ac43f" && \
    export GIT_GPGKEY="E1DD270288B4E6030699E45FA1715D88E1DF1F24" && \
    export DEBIAN_FRONTEND="noninteractive" && \
    export DOTFILES_BASE_URI="https://raw.githubusercontent.com/day8/dockerfiles-for-dev-ci-images/master/dotfiles/" && \
    #
    cd /tmp && \
    #
    # Turn on Bash extended glob support so we can use patterns like !("file1"|"file2")
    shopt -s extglob && \
    #
    apt-get update -qq && \
    apt-get dist-upgrade -qq -y && \
    echo '\n\n' && \
    #
    # Install tools needed by the subsequent commands.
    apt-get install -qq -y --no-install-recommends \
        locales tzdata \
        ca-certificates gnupg \
        curl wget \
        unzip && \
    echo '\n\n' && \
    #
    # Set the default locale to en_AU.UTF-8
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# en_AU.UTF-8 UTF-8/en_AU.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen && \
    echo 'LANG="en_AU.UTF-8"' > /etc/default/locale && \
    dpkg-reconfigure locales && \
    update-locale LANG=en_AU.UTF-8 && \
    #
    # Set the default timezone to Australia/Sydney.
    ln -s --force /usr/share/zoneinfo/Australia/Sydney /etc/localtime && \
    dpkg-reconfigure tzdata && \
    #
    # When adding PPAs do not depend on `software-properties-common` for the `add-apt-repository`
    # command as that adds several hundred megabytes of dependencies.
    echo "Adding 'Git stable releases' PPA..." && \
    echo "deb http://ppa.launchpad.net/git-core/ppa/ubuntu focal main" > /etc/apt/sources.list.d/git-core-ubuntu-ppa-focal.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys $GIT_GPGKEY && \
    echo '\n\n' && \
    #
    # Refresh package lists after adding package repositories.
    apt-get update -qq && \
    #
    # Begin /docker-entrypoint.sh script. This is amended throughout the RUN
    # command with additional lines.
    echo '#!/bin/bash' > /docker-entrypoint.sh && \
    #
    # Install development tooling, common editors and language runtimes.
    apt-get install -qq -y --no-install-recommends \
      git zstd \
      openssh-client \
      build-essential cmake openjdk-$JVM_VERSION\-jdk-headless python2 python3-pip \
    # These are dependencies of xvfb, Chrome and ChromeDriver that are common to all versions.
      xvfb libxcomposite1 libxrandr2 libxss1 libxext6 \
    # Voting in the popularity contest may help keep packages we use supported by Ubuntu.
      popularity-contest && \
    rm -rf /var/lib/apt/lists/* && \
    echo '\n\n' && \
    #
    # Start a X11 virtual framebuffer to run tests in Chrome.
    echo 'Xvfb :99 -screen 0 1920x1080x24 &' >> /docker-entrypoint.sh && \
    #
    # Install Node.js:
    echo "Installing Node.js LTS ${NODEJS_VERSION}..." && \
    wget -q "https://nodejs.org/dist/v${NODEJS_VERSION}/node-v${NODEJS_VERSION}-linux-x64.tar.xz" && \
    echo "Verifying node-v${NODEJS_VERSION}-linux-x64.tar.xz checksum..." && \
    sha256sum node-v${NODEJS_VERSION}-linux-x64.tar.xz && \
    echo "${NODEJS_SHA256SUM} *node-v${NODEJS_VERSION}-linux-x64.tar.xz" | sha256sum -c - && \
    tar --strip-components=1 -C /usr -Jxf "node-v${NODEJS_VERSION}-linux-x64.tar.xz" && \
    rm -f "node-v${NODEJS_VERSION}-linux-x64.tar.xz" && \
    #
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
    #
    # Install Leiningen:
    echo "Installing Leiningen ${LEIN_VERSION}..." && \
    mkdir -p $LEIN_INSTALL && \
    wget -q https://codeberg.org/leiningen/leiningen/raw/tag/$LEIN_VERSION/bin/lein-pkg && \
    echo "Verifying lein-pkg checksum..." && \
    sha256sum lein-pkg && \
    echo "$LEIN_SHA256SUM *lein-pkg" | sha256sum -c - && \
    mv lein-pkg $LEIN_INSTALL/lein && \
    chmod 0755 $LEIN_INSTALL/lein && \
    wget -q "https://codeberg.org/attachments/$LEIN_STANDALONE_JAR" && \
    mkdir -p /usr/share/java && \
    mv $LEIN_STANDALONE_JAR /usr/share/java/leiningen-$LEIN_VERSION-standalone.jar && \
    echo '\n\n' && \
    #
    # Install babashka:
    echo "Installing babashka..." && \
    wget -q "https://github.com/babashka/babashka/releases/download/v${BABASHKA_VERSION}/babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" && \
    echo "Verifying babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz checksum..." && \
    sha256sum "babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" && \
    echo "${BABASHKA_SHA256SUM} *babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" | sha256sum -c - && \
    tar -xzf "babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" -C /usr/local/bin && \
    rm -f "babashka-${BABASHKA_VERSION}-linux-amd64.tar.gz" && \
    bb --version && \
    #
    # Install clj-kondo:
    echo "Installing clj-kondo..." && \
    wget -q "https://github.com/clj-kondo/clj-kondo/releases/download/v${CLJ_KONDO_VERSION}/clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" && \
    echo "Verifying clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip checksum..." && \
    sha256sum "clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" && \
    echo "$CLJ_KONDO_SHA256SUM *clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" | sha256sum -c - && \
    unzip "clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" -d /usr/local/bin && \
    rm -f "clj-kondo-${CLJ_KONDO_VERSION}-linux-amd64.zip" && \
    clj-kondo --version && \
    #
    # Install Terraform:
    echo "Installing Terraform..." && \
    wget -q "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    echo "Verifying terraform_${TERRAFORM_VERSION}_linux_amd64.zip checksum..." && \
    sha256sum "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    echo "${TERRAFORM_SHA256SUM} *terraform_${TERRAFORM_VERSION}_linux_amd64.zip" | sha256sum -c - && \
    unzip "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" -d /usr/local/bin && \
    rm -f "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    strip --strip-unneeded /usr/local/bin/terraform && \
    terraform --version && \
    #
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
    #
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
    #
    ## Install Hasura CLI
    echo "Install Hasura CLI..." && \
    wget -q "https://github.com/hasura/graphql-engine/releases/download/v${HASURA_CLI_VERSION}/cli-hasura-linux-amd64" && \
    echo "Verifying cli-hasura-linux-amd64 checksum..." && \
    sha256sum cli-hasura-linux-amd64 && \
    echo "${HASURA_CLI_SHA256SUM} *cli-hasura-linux-amd64" | sha256sum -c - && \
    mv cli-hasura-linux-amd64 /usr/local/bin/hasura && \
    chmod +x /usr/local/bin/hasura && \
    hasura version && \
    #
    # Install pipenv
    echo "Installing pipenv..." && \
    pip3 -q install pipenv && \
    echo '\n\n' && \
    #
    # Install pytest
    echo "Installing pytest..." && \
    pip3 -q install pytest && \
    echo '\n\n' && \
    #
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
    #
    wget -q -O /root/.gitconfig "${DOTFILES_BASE_URI}/.gitconfig" && \
    #
    # Install NPM-based tools incl Karma CLI & datadog-ci:
    echo "Installing Lumo and Karma CLI..." && \
    npm install -g --unsafe-perm \
      karma-cli@$KARMA_CLI_VERSION \
      @datadog/datadog-ci && \
    #
    # Finish Docker ENTRYPOINT script:
    echo "echo \"`bb --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`lein version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"Node.js `node --version` with NPM `npm --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`python2 --version` and `python3 --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`aws --version` and `sam --version`\"" >> /docker-entrypoint.sh && \
    echo "echo \"`git --version`\"" >> /docker-entrypoint.sh && \
    echo 'exec "$@"' >> /docker-entrypoint.sh && \
    chmod +x /docker-entrypoint.sh && \
    #
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
    #
    # Save about ~22MB by deleting the pip cache:
    rm -rf /root/.cache && \
    #
    # Delete npm's cache:
    rm -rf /root/.npm && \
    #
    # Delete the .m2 repository because we cache this with actions/cache in GitHub Actions:
    rm -rf /root/.m2 && \
    #
    # Cleanup
    rm -rf /usr/share/icons/* \
           /var/lib/apt/lists/*

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/bin/bash"]
