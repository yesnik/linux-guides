# Rsyslog

[Rsyslog](https://www.rsyslog.com/) is the rocket-fast system for log processing. It's a daemon that is capable of forwarding logs to remote servers. 
The configuration makes it possible to centralize log files.

## Config

Config - `/etc/rsyslog.conf`.

Ubuntu `/etc/rsyslog.d/50-default.conf`:

- `*.*;auth,authpriv.none    -/var/log/syslog` - Allow all messages, except `auth`, `authpriv`
- `*.info;mail.none;cron.none /var/log/messages` - Log all `info` messages, except mail, cron
- `kern.*    @rsyslog_server:514` - send all kernel messages to remote server

Message priority:

- emerg
- alert
- crit
- error
- warn
- notice;
- info
- debug

Message level:

- auth
- authpriv
- cron
- daemon
- kern
- lpr
- mail
- mark
- news
- security
- syslog
- user
- uucp
- local0

## Rsyslog modules

Rsyslog has a modular design. This enables functionality to be dynamically loaded from [modules](https://rsyslog.readthedocs.io/en/latest/configuration/modules/index.html), 
which may also be written by any third party. 
Rsyslog itself offers all non-core functionality as modules.

### `im`: Input modules

They are used for input info.

- [imuxsock](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imuxsock.html) - provides the ability to accept syslog messages via local Unix sockets. 
Most importantly, this is the mechanism by which the syslog(3) call delivers syslog messages to rsyslogd. 
So you need to have this module loaded to read the system log socket and be able to process log messages from applications running on the local system.
- [imudp](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) - provides the ability to receive syslog messages via UDP. 

### `om`: Output modules

They are used to output info.

- [omfile](https://rsyslog.readthedocs.io/en/latest/configuration/modules/omfile.html) plug-in provides the core functionality of writing messages to files residing inside the local file system (which may actually be remote if methods like NFS are used). Both files named with static names as well files with names based on message content are supported by this module.
- [omfwd](https://rsyslog.readthedocs.io/en/latest/configuration/modules/omfwd.html) plug-in provides the core functionality of traditional message forwarding via UDP and plain TCP. It is a built-in module that does not need to be loaded.
- [ommysql](https://rsyslog.readthedocs.io/en/latest/configuration/modules/ommysql.html) - provides native support for logging to MySQL databases

### `fm`: Filter modules

They are used for filtering messages.

### `pm`: Parsing modules

They are used for parsing messages.

- [pmciscoios](https://rsyslog.readthedocs.io/en/latest/configuration/modules/pmciscoios.html) - This is a parser that understands Cisco IOS “syslog” format.
  Note that this format is quite different from RFC syslog format, and so the default parser chain cannot deal with it.

## Rsyslog Features

- native support for writing to MariaDB/MySQL databases
- native support for writing to Postgres databases
- ability to monitor text files and convert their contents into syslog messages (one per line)
- [more](https://www.rsyslog.com/doc/features.html)

## Send logs to remote server

We want to send all OS logs (*.*) to remote server 192.168.0.15:9510 using UDP (single `@`) or TCP (double `@@`) in format [RSYSLOG_SyslogProtocol23Format](https://www.rsyslog.com/doc/configuration/templates.html)

Create a file `/etc/rsyslog.d/60-custom.conf`:

```
*.* @192.168.0.15:9510;RSYSLOG_SyslogProtocol23Format
```
Restart rsyslog:

```bash
service rsyslog restart
```

## Configure log host

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
