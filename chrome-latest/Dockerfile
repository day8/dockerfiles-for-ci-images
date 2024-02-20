FROM ghcr.io/day8/core:latest

RUN \
    export CHROME_FOR_TESTING_VERSION="121.0.6167.184" && \
    export CHROME_FOR_TESTING_URL="https://storage.googleapis.com/chrome-for-testing-public/${CHROME_FOR_TESTING_VERSION}/linux64/chrome-linux64.zip" && \
    export CHROME_FOR_TESTING_FILENAME=$(basename "${CHROME_FOR_TESTING_URL%%\?*}") && \
    export CHROMEDRIVER_URL="https://storage.googleapis.com/chrome-for-testing-public/${CHROME_FOR_TESTING_VERSION}/linux64/chromedriver-linux64.zip"  && \
    export CHROMEDRIVER_FILENAME=$(basename "${CHROMEDRIVER_URL%%\?*}") && \
    export CHROME_FOR_TESTING_SHA256SUM="f2496409f3f440c039c967751818480c6029da43b808db307fd2381cabc836ee" && \
    export CHROMEDRIVER_SHA256SUM="a1e864dd3b087733e5c654f57b93a241ee5f9dbf82c6b5a48e7c1768ea9e78e6" && \
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
    wget -q $CHROME_FOR_TESTING_URL && \
    echo "Verifying ChromeDriver package checksum..." && \
    sha256sum $CHROME_FOR_TESTING_FILENAME && \
    echo "$CHROME_FOR_TESTING_SHA256SUM $CHROME_FOR_TESTING_FILENAME" | sha256sum -c - && \
    unzip -q $CHROME_FOR_TESTING_FILENAME && \
    mkdir -p /opt/chrome-for-testing && \
    mv ${CHROME_FOR_TESTING_FILENAME%.*} /opt/chrome-for-testing/$CHROME_FOR_TESTING_VERSION && \
    ln -s /opt/chrome-for-testing/$CHROME_FOR_TESTING_VERSION /opt/chrome-for-testing/latest && \
    ln -s /opt/chrome-for-testing/latest/chrome-wrapper /usr/local/bin/chrome-wrapper && \

    rm -f $CHROME_FOR_TESTING_FILENAME && \
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
    wget -q $CHROMEDRIVER_URL && \
    echo "Verifying ChromeDriver package checksum..." && \
    sha256sum $CHROMEDRIVER_FILENAME && \
    echo "$CHROMEDRIVER_SHA256SUM *$CHROMEDRIVER_FILENAME" | sha256sum -c - && \
    unzip -q $CHROMEDRIVER_FILENAME && \
    rm -f $CHROMEDRIVER_FILENAME && \
    mv ${CHROMEDRIVER_FILENAME%.*}/chromedriver /usr/local/bin/chromedriver && \
    chmod +x /usr/local/bin/chromedriver && \
    chrome-wrapper --version && \
    chromedriver --version && \
    echo '\n\n'
