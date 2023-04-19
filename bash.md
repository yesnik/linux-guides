# Bash

## Variable

**No space around `=`**

```bash
A=1
echo $A
#=> 1
```

**Use double quotes**

```bash
NAME=kenny
echo "/tmp/$NAME"
#=> /tmp/kenny
```

**Use double parenthesis**

```bash
a=2
((result = a + 3))
echo $result
#=> 5
```

**Global and local variables**

```bash
a=1

function add()
{
  local a=3
  local b=4
  (( result = a + b ))
  echo $result

}
add
#=> 7
echo $a
#=> 1
```

## SSH and execute a command

```bash
ssh sales-test "bash -s" << EOF
    tar -zcvf ~/backups/"files-$(date +'%F').tar.gz" -C /var/www/html/public_html ./
EOF
```

This command will create archive `files-2021-03-10.tar.gz` on the remote host.

## Arrays

```bash
arr=(PHP Python JavaScript)

# Count total number of elements of the array
total=${#arr[*]}
echo "Total elements: $total"

# Print each element value of the array
echo "Array values :"
for item in ${arr[*]}
do
  printf "%s\n" $item
done

# Print each element value of the array with key 
echo "Array values with key:"
for key in ${!arr[*]}
do
  printf "%4d: %s\n" $key ${arr[$key]}
done
```

## Program's parameters

We want to get arguments in this program call: `./hey a=1 b`

Edit file `hey`:
```bash
echo "== ARGS =="
for i in "$@"; do
  echo $i
done
```

## Files and dirs

### Get current script dir

Script `/var/tmp/test/current_dir.sh`:

```bash
SCRIPT_DIR="$(dirname "$(realpath "$0")")"
echo $SCRIPT_DIR; # /var/tmp/test
```

## Return code

Script `/tmp/script.sh`:
```bash
exit 4
```

Run:
```bash
sh script.sh
echo $? #=> 4
```
