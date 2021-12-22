# Logs

## Syslog

### Syslog Message Facilities

0 - kernel messages
1 - user-level messages
2 - mail system
3 - system daemons
4 - security/authorization messages
5 - messages generated internally by syslogd

See [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.1)

### Syslog Message Severities

0 - Emergency: system is unusable
1 - Alert: action must be taken immediately
2 - Critical: critical conditions
3 - Error: error conditions
4 - Warning: warning conditions
5 - Notice: normal but significant condition
6 - Informational: informational messages
7 - Debug: debug-level messages

## Rsyslog

*Rsyslog* is an open-source software utility used on UNIX and Unix-like computer systems for forwarding log messages in an IP network. 
It implements the basic *syslog* protocol, extends it with content-based filtering, rich filtering capabilities, queued operations to handle offline outputs, support for different module outputs, flexible configuration options and adds features such as using TCP for transport.

## Links

- The Syslog Protocol [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424)
