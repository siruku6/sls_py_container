# sls_py_container

## Overview

This provides the virtualhost environment which enables you to develop serverless-application with python.

## Description

This container provides followings to you

- node
- npm
- python
- aws-cli
- serverless

## Requirement

You need to install the following packages on your Host OS.

- docker
- docker-compose

## Build Environment

### Prepare .env

```bash
$ cp .env.example .env
```

### Prepare configs for aws-cli (if it is necessary)

```bash
$ cp conf/aws/config.example conf/aws/config
$ cp conf/aws/credentials.example conf/aws/credentials
```

And edit these files.

### Prepare docker-compose.override.yml

```
$ cp docker-compose.yml docker-compose.override.yml
```

And edit docker-compose.override.yml
For example, comment in the section of 'dynamodb'

```bash
$ docker-compose build
$ docker-compose up
```

And then, you can connect port 222 via ssh!

## Customize

You can customize some settings with following Environment Variables on `.env`.

|Variable Name        |Example  |Description|
|:--------------------|--------:|:----------|
|OPEN_PORT            |3000     |If you would like to open any port, for example,<br>which is accessible from your local web browser,<br>you can set the number of that port here.|
|VIRTUAL_HOST_SSH_PORT|222      |This is the number of the port for SSH access.<br>You can access the virtual host machine made of docker container<br>via this port.|
|PERSONAL_ACCESS_TOKEN|ghp_x34x |Set your 'Personal Access Token' for github<br>if you're gonna use the token in virtual host made of docker container.<br>Then it'll be written in `.netrc` automatically<br>when you run `docker-compose build`.|
