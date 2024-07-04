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

## Log Rotate

`logrotat`e is a log managing command-line tool in Linux. 
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
