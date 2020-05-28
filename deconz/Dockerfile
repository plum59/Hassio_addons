ARG BUILD_FROM
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install deCONZ dependencies
ARG BUILD_ARCH
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        iproute2 \
        iputils-ping \
        kmod \
        libcap2-bin \
        libqt5core5a \
        libqt5gui5 \
        libqt5network5 \
        libqt5serialport5 \
        libqt5sql5 \
        libqt5websockets5 \
        libqt5widgets5 \
        lsof \
        netcat \
        nginx \
        sqlite3 \
        tigervnc-common \
        tigervnc-standalone-server \
        udev \
        wget \
        wmii \
        xfonts-base \
        xfonts-scalable \
    && rm -rf /var/lib/apt/lists/* \
    && if [ "${BUILD_ARCH}" = "armhf" ]; \
        then \
            curl -q -L -o /wiringpi.deb https://project-downloads.drogon.net/wiringpi-latest.deb \
            && dpkg -i /wiringpi.deb \
            && rm -rf /wiringpi.deb; \
        fi

# Install deCONZ
ARG DECONZ_VERSION
RUN if [ "${BUILD_ARCH}" = "armhf" ]; \
        then \
            curl -q -L -o /deconz.deb http://www54.uptobox.com/dl/H03qRatR6t1rvOY5GFVGBaWRI-F5BbPHDJskn7ToPFwCjkuQbmUtbqLy6BT42tFAV8MCP1Pht8P5YrURUQLbo1L2OhKRyklzhfXV3WSu4T7k1v4HVTeRSKicEu0dtKVfGDF_zxYF1ju9yVHEiZswjQ/deconz_2.05.78-debian-stretch-beta_arm64.deb; \
        elif [ "${BUILD_ARCH}" = "aarch64" ]; \
        then \
            curl -q -L -o /deconz.deb http://www54.uptobox.com/dl/H03qRatR6t1rvOY5GFVGBaWRI-F5BbPHDJskn7ToPFwCjkuQbmUtbqLy6BT42tFAV8MCP1Pht8P5YrURUQLbo1L2OhKRyklzhfXV3WSu4T7k1v4HVTeRSKicEu0dtKVfGDF_zxYF1ju9yVHEiZswjQ/deconz_2.05.78-debian-stretch-beta_arm64.deb; \
        else \
            curl -q -L -o /deconz.deb http://www54.uptobox.com/dl/H03qRatR6t1rvOY5GFVGBaWRI-F5BbPHDJskn7ToPFwCjkuQbmUtbqLy6BT42tFAV8MCP1Pht8P5YrURUQLbo1L2OhKRyklzhfXV3WSu4T7k1v4HVTeRSKicEu0dtKVfGDF_zxYF1ju9yVHEiZswjQ/deconz_2.05.78-debian-stretch-beta_arm64.deb; \
        fi \
    && dpkg -i /deconz.deb \
    && rm -f /deconz.deb \
    && chown root:root /usr/bin/deCONZ* \
    && sed -i 's/\/root/\/data/' /etc/passwd

COPY data/ika-otau-dl.sh /bin/
COPY data/ledvance-otau-dl.sh /bin/
COPY data/nginx.conf /etc/nginx/nginx.conf
COPY data/run.sh data/discovery.sh /

CMD ["/run.sh"]
