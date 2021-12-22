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

*Rsyslog* is a daemon that is capable of forwarding logs to remote servers. The configuration makes it possible to centralize log files.

### Configure log host

*Log host* is a server hosting log files. 

1. Mount `/var/log` on a separate partition. It's critical when the server receives log files from many servers.
2. Edit `/etc/rsyslog.conf` or file at `/etc/rsyslog.d/`. Uncomment lines to enable UDP:
```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")
```
3. Configure a [template](https://www.rsyslog.com/doc/v8-stable/configuration/templates.html) for incoming logs. 
  If we don't configure a template, all of the log entries from the remote servers mix with the log host's logs.
```
$template DynamicFile,"/var/log/%HOSTNAME%/forwarded-logs.log" 
*.* -?DynamicFile
```
4. Restart `systemctl restart rsyslog`
5. Verify that rsyslog is listening port 514: `ss -4altunp | grep 514`
6. Allow rsyslog through firewall: `ufw allow 514/udp`
7. To allow specific hosts for either UDP or TCP logging, edit `rsyslog.conf`:
    ```
    # $AllowedSender - define which remote servers are allowed to send syslog messages to our rsyslogd
    $AllowedSender UDP, 192.168.51.0/24, [::1]/128, *.site.org, s1.ex.com
    $AllowedSender TCP, 192.168.52.0/24, [::1]/128, *.site.org, s2.ex.com
    ```

## Links

- The Syslog Protocol [RFC 5424](https://datatracker.ietf.org/doc/html/rfc5424)
