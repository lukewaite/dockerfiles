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
	libffi-dev

RUN \
	apk add --no-cache yaml-dev
ENV SAM_VERSION=1.21.1
RUN cd /root && git clone --depth 1 --branch v${SAM_VERSION} https://github.com/awslabs/aws-sam-cli.git && cd aws-sam-cli && python setup.py install


RUN mkdir -p /root/.aws \
	&& { \
		echo '[default]'; \
		echo 'output = json'; \
		echo 'region = $AWS_DEFAULT_REGION'; \
		echo 'aws_access_key_id = $AMAZON_ACCESS_KEY_ID'; \
		echo 'aws_secret_access_key = $AMAZON_SECRET_ACCESS_KEY'; \
	} > /root/.aws/config \
    && apk del build-deps

ENV HOME /root
WORKDIR $HOME/aws-sam-workspace

ENTRYPOINT [ "sam" ]