# Ubuntu proxy settings

If your PC behind proxy you need to configure settings to access Internet.

## Does wget command work?

```
wget google.com
```

If it doesn't work edit file `/etc/environment`:

```
export http_proxy=http://username:password@proxyserver.com:port
export https_proxy=http://username:password@proxyserver.com:port
export no_proxy=127.0.0.1,localhost
```

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
