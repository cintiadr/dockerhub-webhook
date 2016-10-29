[![Build Status](https://travis-ci.org/maccyber/dockerhub-webhook.svg?branch=master)](https://travis-ci.org/maccyber/dockerhub-webhook)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://github.com/feross/standard)
[![Coverage Status](https://coveralls.io/repos/github/maccyber/dockerhub-webhook/badge.svg)](https://coveralls.io/github/maccyber/dockerhub-webhook)

# dockerhub-webhook

Automatic Docker Deployment via [Webhooks](https://docs.docker.com/docker-hub/builds/#webhooks).

dockerhub-webhook listens to incoming HTTP requests from hub.docker.com and triggers your specified script.

## Features

* Lightweight
* Pretty simple setup process
* Supports updating multiple docker images
* Scripts can trigger docker or docker-compose

# Create secret
Create a secret token with ``openssl``, ``uuidgen`` or something else. Remember not to use any slashes since it's going to be used in the URL.

```sh
export TOKEN=$(openssl rand -base64 30 | sed 's=/=\\/=g')
echo $TOKEN
```

# Installation alternatives

## 1. Run on host

### Install

Nodejs and npm must be installed.

```sh
git clone http://github.com/maccyber/dockerhub-webhook
cd dockerhub-webhook
npm i
```

### Edit config
```sh
vim config/index.js
```

### Edit webhooks and scripts
```sh
vim scripts/index.js
```

### Test
```sh
npm start
```

## 2. Run with docker-compose

Git clone
```sh
git clone http://github.com/maccyber/dockerhub-webhook
```

Add secret token in dockerhub.env with
```sh
vim dockerhub.env
```

Start with
```sh
docker-compose up -d
```

## 3. Run from docker hub

Git clone
```sh
git clone http://github.com/maccyber/dockerhub-webhook
```

Start with
```sh
docker run -d \
  -p 3000:3000 \
  -e SERVER_PORT=3000 \
  -e TOKEN=abc123 \
  -e ROUTE=/api \
  -v ${PWD}/scripts:/src/scripts \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name dockerhub-webhook \
  maccyber/dockerhub-webhook 
```

# Configuration on docker hub

Go to https://hub.docker.com/ -> your repo -> Webhooks

Add a webhook like on the following image.

![alt tag](dockerhook.png)

``example.com`` can be the domain of your server or its ip address.

docker-hook listens to port 3000. Please replace abc123 with your safe auth-token.
