FROM ghcr.io/day8/core:latest

ENV CHROME_BIN=/usr/local/bin/chrome-wrapper

RUN \
    export CHROMIUM_VERSION="56.0.2924.0" && \
    export CHROMIUM_PACKAGE_URL="https://www.googleapis.com/download/storage/v1/b/chromium-browser-snapshots/o/Linux_x64%2F433062%2Fchrome-linux.zip?generation=1479441205933000&alt=media" && \
    export CHROMIUM_SHA256SUM="5e2e38ffbed6ff9bef287026c8d2628b12b5a43b5bf7c235bfa182e7fd5d213f" && \
    export CHROMEDRIVER_VERSION="2.29" && \
    export CHROMEDRIVER_SHA256SUM="bb2cf08f2c213f061d6fbca9658fc44a367c1ba7e40b3ee1e3ae437be0f901c2" && \

    export DEBIAN_FRONTEND="noninteractive" && \

    cd /tmp && \

    # Turn on Bash extended glob support so we can use patterns like !("file1"|"file2")
    shopt -s extglob && \

    # Refresh package lists.
    apt-get update -qq && \

    # These are dependencies of xvfb and/or Chrome. Unfortunately due to the old version of Chrome
    # used (56.x) there is a dependency on old libraries like libgtk2.0-0 that take up a lot of
    # extra space.
    apt-get install -qq -y --no-install-recommends \
       libxcursor1 libgtk2.0-0 libgconf-2-4 libxtst6 && \

    rm -rf /var/lib/apt/lists/* && \

    # Install Chrome
    #
    # We have an Electron app that uses an older Chrome version. So we want to test with that same older version of
    # Chrome in this image. Google deletes old versions every release, so we can't get it from their official deb package
    # repository. Instead, jump through the following hoops using an example of Chrome 56.x:
    #
    # 1. Look in https://chromereleases.googleblog.com/ for the last time "56." was mentioned in a Desktop release. In
    #    this case I found https://chromereleases.googleblog.com/2017/01/stable-channel-update-for-desktop.html
    #    NOTE: I had to use '56.0.2924.77' instead of '56.0.2924.87' as '56.0.2924.87' does not return a base position
    #          in step 2.
    #
    # 2. Look up that version history ('56.0.2924.77') in the position lookup: https://omahaproxy.appspot.com/
    #    In this case it returns a 'Branch Base Position' of '433059'.
    #
    # 3. Open the continuous build archive: https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html
    #
    # 4. Click 'Linux_x64'
    #
    # 5. Paste the 'Branch Base Position' ('433059') into the 'Filter:' field at the top and wait for remote network
    #    requests to complete. Sometimes you need to decrement or increment the number until you find a match.
    #
    # 6. After waiting a long time you might get a hit; e.g. for '433062' I got:
    #      https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Linux_x64/433062/
    #
    # 7. Copy/paste the download URL for 'chrome-linux.zip'.
    echo "Installing Chromium ${CHROMIUM_VERSION}..." && \
    wget -q -O chromium-linux.zip $CHROMIUM_PACKAGE_URL && \
    echo "Verifying chromium-linux.zip checksum..." && \
    sha256sum chromium-linux.zip && \
    echo "$CHROMIUM_SHA256SUM *chromium-linux.zip" | sha256sum -c - && \
    unzip -q chromium-linux.zip && \
    mkdir -p /opt/chromium && \
    mv chrome-linux /opt/chromium/$CHROMIUM_VERSION && \
    ln -s /opt/chromium/$CHROMIUM_VERSION /opt/chromium/latest && \
    ln -s /opt/chromium/latest/chrome-wrapper /usr/local/bin/chrome-wrapper && \
    # Fixes error on startup like:
    # 'ChromeHeadless stderr: [3284:3284:0816/000443:FATAL:zygote_host_impl_linux.cc(107)] No usable sandbox! Update your kernel...'
    #
    # TODO When upgrading Chrome: Verify that there are no changes to `chrome-wrapper` that would cause this to fail.
    sed -i 's|HERE/chrome"|HERE/chrome" --disable-setuid-sandbox --no-sandbox|g' \
        /opt/chromium/$CHROMIUM_VERSION/chrome-wrapper && \
    rm chromium-linux.zip && \

    # Save about ~18MB by deleting all Chromium locales except English:
    #rm -f /opt/chromium/latest/locales/!(en-GB.pak|en-US.pak) && \

    # Install older libraries required by older Chrome release (56.x).
    #
    # The version of Chrome installed is not compatible with newer releases of
    # the following libraries:
    #     $ ldd chrome | grep pango
    #       libpangocairo-1.0.so.0 => /lib/x86_64-linux-gnu/libpangocairo-1.0.so.0 (0x00007f314a52b000)
    #       libpango-1.0.so.0 => /lib/x86_64-linux-gnu/libpango-1.0.so.0 (0x00007f314a2de000)
    #       libpangoft2-1.0.so.0 => /lib/x86_64-linux-gnu/libpangoft2-1.0.so.0 (0x00007f3148673000)
    #
    # This issue causes an error like:
    #     (chrome:1733): Pango-ERROR **: 04:57:48.341: Harfbuzz version too old (1.3.1)
    #
    # To fix it older versions of the associated packages are installed over the top of the current
    # versions.
    #
    # TODO When upgrading Chrome: Remove this fix and verify the above error does not reoccur.
    #
    # Since this fix is expected to be removed when upgrading Chrome, the specific versions and
    # checksums are not stored in environment variables above for ease of upgrades.
    wget -q "http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "Verifying libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb checksum..." && \
    sha256sum "libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "bbe6059047b0e03433aee358b01d12c8dc083532fbd8160494aeb9dc74f3a3d9 *libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" | sha256sum -c - && \
    dpkg -i "libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    rm -f "libpango-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \

    wget -q "http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "Verifying libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb checksum..." && \
    sha256sum "libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "cb0f273b2ae6f752aa8d295aedb38eb4820cffc2c0c7fc25bc341ec8b182930a *libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" | sha256sum -c - && \
    dpkg -i "libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    rm -f "libpangocairo-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \

    wget -q "http://mirrors.kernel.org/ubuntu/pool/main/p/pango1.0/libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "Verifying libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb checksum..." && \
    sha256sum "libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    echo "e64b05c5108e501745955288a19492876f2bfbc7f54497fd6b87ae7b8b2238eb *libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" | sha256sum -c - && \
    dpkg -i "libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \
    rm -f "libpangoft2-1.0-0_1.40.14-1ubuntu0.1_amd64.deb" && \

    echo '\n\n' && \

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
    chrome-wrapper --version && \
    chromedriver --version && \
    echo '\n\n'
