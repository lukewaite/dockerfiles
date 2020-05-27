FROM golang:alpine as builder

ENV FARGATE_VERSION=0.3.2
RUN \
    wget https://github.com/awslabs/fargatecli/archive/${FARGATE_VERSION}.zip \
    && unzip ${FARGATE_VERSION}.zip \
    && cd fargatecli-${FARGATE_VERSION}\
    && go build -ldflags="-s -w" -o /go/fargate

FROM alpine

COPY --from=builder /go/fargate /usr/local/bin

ENTRYPOINT ["fargate"]