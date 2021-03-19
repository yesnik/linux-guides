# Bash Syntax

## Variable

```bash
NAME=kenny

echo "/tmp/${NAME}"
```

## SSH and execute a command

```bash
ssh sales-test "bash -s" << EOF
    tar -zcvf ~/backups/"files-$(date +'%F').tar.gz" -C /var/www/html/public_html ./
EOF
```

This command will create archive `files-2021-03-10.tar.gz` on the remote host.
