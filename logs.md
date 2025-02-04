# Logs

## Log systems

### Syslog

#### Syslog Message Facilities

0 - kernel messages
1 - user-level messages
2 - mail system
3 - system daemons
4 - security/authorization messages
5 - messages generated internally by syslogd

See [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.1)

#### Syslog Message Severities

0 - Emergency: system is unusable
1 - Alert: action must be taken immediately
2 - Critical: critical conditions
3 - Error: error conditions
4 - Warning: warning conditions
5 - Notice: normal but significant condition
6 - Informational: informational messages
7 - Debug: debug-level messages

### Logstash

[Logstash](https://www.elastic.co/logstash) is an open source server-side data processing pipeline that ingests data from a multitude of sources, transforms it, and then sends it to your favorite "stash.".
It's written in JRuby, JVM required.

**ELK** — Elastic Search, Logstash, Kibana.

### Filebeat

[Filebeat](https://www.elastic.co/beats/filebeat) is a lightweight shipper for forwarding and centralizing log data. 
Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, 
collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Filebeat is a part of ELK stack.

- It can reduce reading speed if Elastic is under heavy load.
- Focused on reliability

### Fluentd

[Fluentd](https://www.fluentd.org/) is an open source data collector for unified logging layer.
Fluentd allows you to unify data collection and consumption for a better use and understanding of data.

### Fluent Bit

[Fluent Bit](https://fluentbit.io/) is a super fast, lightweight, and highly scalable logging, metrics, and traces processor and forwarder. 
It is the preferred choice for cloud and containerized environments.

### Logs example

- To display boot and other kernel messages: `tail -f /var/log/messages`
- To view users and their activities: `tail -f /var/log/messages`

## Log Rotate

`logrotate` is a log managing command-line tool in Linux. 

The administrators write the rules and policies for handling different log files into configuration files. 
Through the configuration file, logrotate will execute the appropriate function to manage the matching log files.

Configs: `/etc/logrotate.conf`, `/etc/logrotate.d/*`

```bash
logrotate --help
```

Example of config file `/etc/logrotate.d/bootlog`:

```
/var/log/boot.log
{
    missingok
    daily
    copytruncate
    rotate 7
    notifempty
}
```

- `missingok` - If the log file is missing, go on to the next one without issuing an error message. 
- `daily` - Log files are rotated every day.
- `copytruncate` - Truncate the original log file in place after creating a copy, instead of moving the old log file and optionally creating a new one.
- `rotate 7` - Log files are rotated 7 times before being removed or mailed to the address specified in a mail directive.
- `notifempty` - Do not rotate the log if it is empty

See another [options](https://linux.die.net/man/8/logrotate)

## Links

- The Syslog Protocol [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424)
