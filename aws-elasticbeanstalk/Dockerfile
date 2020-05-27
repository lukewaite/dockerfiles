FROM python:3-alpine

RUN \
    apk --no-cache add \
	groff \
	less \
	git \
	openssh-client

RUN \
	apk add --no-cache --virtual build-deps \
	build-base \
	python3-dev \
	openssl-dev \
	libffi-dev \
    && pip3 install awsebcli==3.17.1 \
	&& mkdir -p /root/.aws \
	&& { \
		echo '[default]'; \
		echo 'output = json'; \
		echo 'region = $AWS_DEFAULT_REGION'; \
		echo 'aws_access_key_id = $AMAZON_ACCESS_KEY_ID'; \
		echo 'aws_secret_access_key = $AMAZON_SECRET_ACCESS_KEY'; \
	} > /root/.aws/config \
    && apk del build-deps

RUN apk add docker-cli

ENV HOME /root
WORKDIR $HOME/aws-elasticbeanstalk-workspace

ENTRYPOINT [ "eb" ]