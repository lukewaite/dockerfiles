ARG GO_VERSION=1.16.3
ARG REPO_NAME=""
ARG APP_NAME="iamlive"
ARG APP_PATH="/go/src/iamlive"
ARG IAMLIVE_VERSION=0.41.0

# Dev
FROM golang:${GO_VERSION}-alpine AS dev
RUN apk add --update git
ARG APP_NAME
ARG APP_PATH
ENV APP_NAME="${APP_NAME}" \
    APP_PATH="${APP_PATH}" \
    GOOS="linux"
WORKDIR "${APP_PATH}"
COPY . "${APP_PATH}"
ENTRYPOINT ["sh"]

# Fetch
FROM alpine:3.12 as fetch
ARG IAMLIVE_VERSION
RUN apk add --update wget
RUN wget https://github.com/iann0036/iamlive/archive/refs/tags/v${IAMLIVE_VERSION}.zip
RUN unzip v${IAMLIVE_VERSION}.zip -d /tmp

# Build
FROM dev as build
ARG IAMLIVE_VERSION
COPY --from=fetch /tmp/iamlive-${IAMLIVE_VERSION} /go/src/iamlive
RUN go install
ENTRYPOINT [ "sh" ]

# App
FROM alpine:3.12 AS app
RUN apk --update upgrade && \
    apk add --update ca-certificates && \
    update-ca-certificates
WORKDIR "/app/"
COPY --from=build "/go/bin/iamlive" ./iamlive
RUN addgroup -S "appgroup" && adduser -S "appuser" -G "appgroup" && \
    chown "appuser:appgroup" "./iamlive"

USER "appuser"
EXPOSE 10080
ENTRYPOINT ["./iamlive"]
CMD ""