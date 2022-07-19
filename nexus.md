# Nexus

## Config Docker with Nexus

To use Nexus instead of default Docker registry (https://registry-1.docker.io/), edit `/etc/docker/daemon.json`:

```
{
  "registry-mirrors": ["https://nexusproxy.lan.example.com"]
}
```
