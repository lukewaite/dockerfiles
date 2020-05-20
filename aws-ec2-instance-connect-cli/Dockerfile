FROM lukewaite/awscli

RUN \
    apk add --no-cache --virtual build-deps build-base openssl-dev libffi-dev python3-dev \
    && apk add --no-cache openssh-client \
    && pip3 install ec2instanceconnectcli \
    && apk del build-deps

ENTRYPOINT [ "mssh" ]
