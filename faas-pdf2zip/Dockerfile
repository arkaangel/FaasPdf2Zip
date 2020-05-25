FROM openfaas/classic-watchdog:0.18.1 as watchdog

FROM alpine:3.11

RUN mkdir -p /home/app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

#instalo las dependencia para trabajar el pdf y convertirlo a img
RUN apk add --no-cache poppler-utils \
    curl \
    zip \
    msttcorefonts-installer && \
    update-ms-fonts 2>&1 && \
    fc-cache -f

COPY ./fprocess /usr/local/bin
RUN chmod +x /usr/local/bin/fprocess


# Add non root user
RUN addgroup -S app && adduser app -S -G app
RUN chown app /home/app

WORKDIR /home/app

#USER app

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="xargs fprocess"
# Set to true to see request in function logs
ENV write_debug="false"

EXPOSE 8080

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
