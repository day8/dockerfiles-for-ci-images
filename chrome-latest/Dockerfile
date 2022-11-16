FROM ghcr.io/day8/core:latest

RUN \
    export CHROME_VERSION="107.0.5304.110-1" && \
    export CHROME_SHA256SUM="52e6d1be974c36cb40469f03b128cf21b36359a05fdadbb2e0379fbb17ef341b" && \
    export CHROMEDRIVER_VERSION="107.0.5304.62" && \
    export CHROMEDRIVER_SHA256SUM="502f124092160cbfde42705cb24aad8b48b4b5c6c22b440a34d83034019d408f" && \
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
