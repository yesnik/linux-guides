# Service Discovery

Zabbix is good for monitoring servers that is used for several years.

Prometheus is good for monitoring container apps, where targets for monitoring can be added automatically.

See [config docs](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)

### kubernetes_sd_config

Kubernetes SD configurations allow retrieving scrape targets from Kubernetes' REST API and always staying synchronized with the cluster state.

One of the following role types can be configured to discover targets:

- `node`. The node role discovers one target per cluster node with the address defaulting to the Kubelet's HTTP port.
- `service`. The service role discovers a target for each service port for each service. This is generally useful for blackbox monitoring of a service.
  The address will be set to the Kubernetes DNS name of the service and respective service port.
- `pod`. The pod role discovers all pods and exposes their containers as targets. For each declared port of a container, a single target is generated. 

### consul_sd_config

Consul SD configurations allow retrieving scrape targets from [Consul](https://www.consul.io/)'s Catalog API.

When a new application registers in Consul, Prometheus starts monitoring it.

### dns_sd_config

A DNS-based service discovery configuration allows specifying a set of DNS domain names which are periodically queried to discover a list of targets. 
The DNS servers to be contacted are read from `/etc/resolv.conf`.

This service discovery method only supports basic DNS A, AAAA, MX, NS and SRV record queries.

#### "SRV" DNS-record

Edit `prometheus.yml`:

```yml
  - job_name: 'dns'
    dns_sd_configs:
    - names:
      - _hey._tcp.example.com
      type: SRV
      refresh_interval: 60s

```

#### "A" DNS-record

Edit `prometheus.yml`:

```yml
  - job_name: 'dns'
    dns_sd_configs:
    - names:
      - example.com
      type: A
      port: 9090
      refresh_interval: 15s
```

In this case we define all IP addresses that belong to domain example.com. We connect to them on port 9090.

### file_sd_config

File-based service discovery provides a more generic way to configure static targets and serves as an interface to plug in custom service discovery mechanisms.

It reads a set of files containing a list of zero or more `<static_config>`s. 
Changes to all defined files are detected via disk watches and applied immediately.

While those individual files are watched for changes, the parent directory is also watched implicitly. 
This is to handle atomic renaming efficiently and to detect new files that match the configured globs. 
This may cause issues if the parent directory contains a large number of other files, 
as each of these files will be watched too, even though the events related to them are not relevant.

Files may be provided in `YAML` or `JSON` format. Only changes resulting in well-formed target groups are applied.

## Apply to Prometheus

Edit `prometheus.yml`:

```yml
  - job_name: 'file'
    file_sd_configs:
    - files:
      - '/opt/targets/*.json'
      refresh_interval: 10s
```

Create folder for json files: `mkdir -p /opt/targets`.

Any service can put json files in this folder, and Prometheus will start to collect metrics from these targets.

See Service Discovery Web Page at Prometheus Web interface: http://<ip-address>:9090/

Create `/opt/target/example.json`:

```json
[
    {
        "targets": [ "10.0.0.2:9100" ],
        "labels": {
            "env": "prod"
        }
    }
]
```
