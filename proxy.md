# Proxy settings

If your PC with Ubuntu behind proxy you need to configure settings to access Internet.

## Does wget command work?

```
wget google.com
```

If it doesn't work edit file `/etc/environment`:

```
export http_proxy=http://username:password@proxyserver.com:port
export https_proxy=http://username:password@proxyserver.com:port
export no_proxy=127.0.0.1,localhost

export HTTP_PROXY=http://username:password@proxyserver.com:port
export HTTPS_PROXY=http://username:password@proxyserver.com:port
export NO_PROXY=127.0.0.1,localhost
```

Uppercase variables (`HTTP_PROXY`, `HTTPS_PROXY`) may be required by some programms (e.g. Docker). 

To apply changes, **reboot** your PC.

## Can you install any package?

```
sudo apt install mc
```

You can get an error `407 Proxy Authentication Required [IP: 177.18.6.233]`.

To fix this error edit `/etc/apt/apt.conf`:

```
Acquire::http::Proxy "http://username:password@proxyserver.com:port";
Acquire::https::Proxy "http://username:password@proxyserver.com:port";
```

## Docker proxy settings

For more info see [docs](https://docs.docker.com/config/daemon/systemd/).

You need to config your proxy if this command returns error:

```
docker-compose up
Building fpm
Step 1/5 : FROM php:fpm
ERROR: Service 'fpm' failed to build: Get https://registry-1.docker.io/v2/: Proxy Authentication Required
```

### Way 1

- Edit `/etc/environment` file and define there uppercase variables `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY`
- Reboot PC

### Way 2

1. Create or edit file `/etc/systemd/system/docker.service.d/http-proxy.conf`:

```
[Service]
Environment="HTTP_PROXY=http://johny:mypassword@177.17.1.233:3121"
Environment="HTTPS_PROXY=http://johny:mypassword@177.17.1.233:3121"
Environment="NO_PROXY=localhost,127.0.0.1,localaddress,.localdomain.com"
```

2. Flush changes

```
sudo systemctl daemon-reload
```

3. Restart Docker

```
sudo systemctl restart docker
```

## NPM proxy settings

Try to install package:

```
npm install -g @vue/cli

npm ERR! code E407
npm ERR! 407 Proxy Authentication Required: @vue/cli@latest
```

**Get current NPM proxy config**

```
npm config get proxy
```

**Set new NPM proxy config**

```
npm config set proxy "http://myuser:mypassword@150.17.5.111:3128/"
```

It this didn't help, try to change registry to `http`:

```
npm config rm proxy
npm config rm https-proxy
npm config set registry http://registry.npmjs.org/
```

## Git proxy settings

By default GIT will use system proxy settings defined in `/etc/environment`.

Ensure that your GIT don't override these settings: `~/.gitconfig`

