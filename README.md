# dev_docker_py3

## Overview

This provides the virtualhost environment which enables you to develop apps with python.

## Description

## Requirement

You need to install the following packages on your OS.

- docker
- docker-compose

## Deployment

```bash
$ cp .env.example .env

$ cp docker-compose.yml docker-compose.override.yml

# Edit docker-compose.override.yml
# For example, comment in the section of 'dynamodb'

$ docker-compose build
$ docker-compose up
```

And then, you can connect port 222 via ssh!
That provides the environment for development with python.

## Customize

You can customize some settings with following Environment Variables on `.env`.

|Variable Name        |Example  |Description|
|:--------------------|--------:|:----------|
|PYTHON_VERSION       |3.8.1    |Set the version of python which you would like to use.|
|OPEN_PORT            |3000     |If you would like to open any port, for example,<br>which is accessible from your local web browser,<br>you can set the number of that port here.|
|VIRTUAL_HOST_SSH_PORT|222      |This is the number of the port for SSH access.<br>You can access the virtual host machine made of docker container<br>via this port.|
|PERSONAL_ACCESS_TOKEN|ghp_x34x |Set your 'Personal Access Token' for github<br>if you're gonna use the token in virtual host made of docker container.<br>Then it'll be written in `.netrc` automatically<br>when you run `docker-compose build`.|
