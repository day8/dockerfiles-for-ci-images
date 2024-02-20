FROM ghcr.io/day8/core:latest

# Launcher Electron Chrome version 87.0.4280.141 from Wednesday, January 6, 2021
#

RUN \
    export CHROMIUM_VERSION="87" && \
    export CHROMIUM_SHA256SUM="36759ed6d151645d00a3a015200334edc70188b422eec51bcaa5790c8e906e27" && \
    export CHROMIUM_PACKAGE_URL="https://www.googleapis.com/download/storage/v1/b/chromium-browser-snapshots/o/Linux_x64%2F812859%2Fchrome-linux.zip?generation=1601581775200441&alt=media" && \
    export CHROMEDRIVER_VERSION="87" && \
    export CHROMEDRIVER_SHA256SUM="d859f8ecb21e26d3ddaf3f229da695bc86512f4e6c9fe32533af7a8b36783ec5" && \
    export CHROMEDRIVER_PACKAGE_URL="https://www.googleapis.com/download/storage/v1/b/chromium-browser-snapshots/o/Linux_x64%2F812859%2Fchromedriver_linux64.zip?generation=1601581780133920&alt=media"

    #
    cd /tmp && \
    #
    # Turn on Bash extended glob support so we can use patterns like !("file1"|"file2")
    shopt -s extglob && \
    #
    # Refresh package lists.
    apt-get update -qq && \
    #
    apt-get install -qq -y --no-install-recommends \
      # These are dependencies of Chrome and ChromeDriver that are not common between the different
      # versions.
      fonts-liberation libatk-bridge2.0-0 libatk1.0-0 libatspi2.0-0 libcairo2 libgbm1 libgtk-3-0 \
      libpango-1.0-0 libxdamage1 libxkbcommon0 xdg-utils && \
    #
    rm -rf /var/lib/apt/lists/* && \
    #
    wget -q -O chromium-linux.zip $CHROMIUM_PACKAGE_URL && \
    echo "Verifying Chrome package checksum..." && \
    sha256sum chromium-linux.zip && \
    echo "$CHROMIUM_SHA256SUM *chromium-linux.zip" | sha256sum -c - && \
    unzip -q chromium-linux.zip && \
    mkdir -p /opt/chromium && \
    mv chrome-linux /opt/chromium/$CHROMIUM_VERSION && \
    ln -s /opt/chromium/$CHROMIUM_VERSION /opt/chromium/latest && \
    ln -s /opt/chromium/latest/chrome-wrapper /usr/local/bin/chrome-wrapper && \


    #
    # Install ChromeDriver
    #
    # ChromeDriver version MUST be the correct version for the Chrome release it is being used with.
    #
    # For recent versions of Chrome see https://chromedriver.chromium.org/downloads/version-selection
    #
    # For older versions of Chrome (e.g. '56.x') look through https://chromedriver.storage.googleapis.com/index.html
    # and find the newest release with a 'notes.txt' file that mentions the major version e.g. 'Supports Chrome v56-58'.
    echo "Installing ChromeDriver ${CHROMEDRIVER_VERSION}..." && \
    wget -q -O chromedriver_linux64.zip $CHROMEDRIVER_PACKAGE_URL && \
    echo "Verifying ChromeDriver package checksum..." && \
    sha256sum chromedriver_linux64.zip && \
    echo "$CHROMEDRIVER_SHA256SUM *chromedriver_linux64.zip" | sha256sum -c - && \
    unzip -q chromedriver_linux64.zip && \
    rm -f chromedriver_linux64.zip && \
    mv chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    chrome-wrapper --version && \
    chromedriver --version && \
    echo '\n\n'
