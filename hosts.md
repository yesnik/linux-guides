# File /etc/hosts

On CentOS Linux you can find file `/etc/hosts`. Let's add this line there:

```
173.194.220.113 hello.ru
```

Then we'll try to visit *hello.ru* from this PC:

```
ping hello.ru
PING hello.ru (173.194.220.113) 56(84) bytes of data.
64 bytes from hello.ru (173.194.220.113): icmp_seq=1 ttl=47 time=15.6 ms
```

So this file allows us to redirect all *hello.ru* requests to IP 173.194.220.113.
