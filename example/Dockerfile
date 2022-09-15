# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM

FROM bettercap/bettercap AS bettercap-base

FROM $BUILD_FROM
RUN apk add apparmor

# Execute during the build of the image
ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

RUN apk add --no-cache ca-certificates
RUN apk add --no-cache bash iproute2 libpcap libusb-dev libnetfilter_queue wireless-tools

# Copy root filesystem
COPY rootfs /
COPY --from=bettercap-base /app/ /app/
RUN /app/bettercap -eval "caplets.update; ui.update; q"