FROM ghcr.io/day8/core:latest

# Launcher Electron Chrome version 87.0.4280.141 from Wednesday, January 6, 2021
#

RUN \
    export CHROME_VERSION="87.0.4280.141" && \
    export CHROME_SHA256SUM="e8c00b766c50b57898c0084f41eac7642b527b591bd079dc8191d04352260b34" && \
    export CHROMEDRIVER_VERSION="87.0.4280.88" && \
    export CHROMEDRIVER_SHA256SUM="929a3b4246742a842fd49391dddde6e829cb7f2e94aed92091ba88586380dac6" && \
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
    wget -q "https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_VERSION}_amd64.deb" && \
    echo "Verifying ChromeDriver package checksum..." && \
    sha256sum "google-chrome-stable_${CHROME_VERSION}_amd64.deb" && \
    echo "$CHROME_SHA256SUM *google-chrome-stable_${CHROME_VERSION}_amd64.deb" | sha256sum -c - && \
    dpkg -i "google-chrome-stable_${CHROME_VERSION}_amd64.deb" && \
    rm -f "google-chrome-stable_${CHROME_VERSION}_amd64.deb" && \
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
    wget -q https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip && \
    echo "Verifying ChromeDriver package checksum..." && \
    sha256sum chromedriver_linux64.zip && \
    echo "$CHROMEDRIVER_SHA256SUM *chromedriver_linux64.zip" | sha256sum -c - && \
    unzip -q chromedriver_linux64.zip && \
    rm -f chromedriver_linux64.zip && \
    mv chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    /opt/google/chrome/chrome --version && \
    chromedriver --version && \
    echo '\n\n'
