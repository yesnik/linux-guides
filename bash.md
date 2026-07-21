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

## `if`, `else` condition

### Compare a variable with a value

```bash
NAME="kenny"
if [ $NAME = "kenny" ]
then
    echo "Hello Kenny!"
else
    echo "Bye!"
fi
```

### Check if empty

```bash
NAME=$1
if [ -z $NAME ]
then
    echo "Bye"
else
    echo "Hello $NAME"
fi
```

Test:

```bash
./script.sh
Bye

./script.sh Kenny
Hello Kenny
```

### Compare numbers

Use operators:

- `-eq` - `=`
- `-ne` - `!=`
- `-lt` - `<`
- `-le` - `<=`
- `-gt` - `>`
- `-ge` - `>=`

```bash
SPEED=$1
if [ $SPEED -lt 5  ]
then
    echo "Slow"
fi

if [ $SPEED -eq 6 ]
then
    echo "Move faster"
fi

if [ $SPEED -ge 200 ]
then
    echo "Danger"
fi
```
Test:

```bash
./script.sh 2
Slow
./script.sh 6
Move faster
./script.sh 200
Danger
```

### Check if process is running

```bash
if prep -f "clientDataConsumer" > /dev/null
then
    echo "Process is running"
else
    echo "No process"
fi
```

### SSH and execute a command

```bash
ssh sales-test "bash -s" << EOF
    tar -zcvf ~/backups/"files-$(date +'%F').tar.gz" -C /var/www/html/public_html ./
EOF
```

This command will create archive `files-2021-03-10.tar.gz` on the remote host.

## Loop

```bash
for i in {1..4}
do
    echo "Hello $i"
done
```

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

We want to get arguments in this program call: `./script.sh 11 b=22 c=33`

File `script.sh`:

```bash
#! /bin/bash

echo "First arg: $1"
echo "Second arg: $2"
echo "Args count: $#"

for arg in "$@"; do
    echo "Arg: $arg"
done
```

Test:

```bash
./script.sh 11 b=22 c=33
First arg: 11
Second arg: b=22
Args count: 3
Arg: 11
Arg: b=22
Arg: c=33
```

## Files and dirs

### Get current script dir

Script `/var/tmp/test/current_dir.sh`:

```bash
SCRIPT_DIR="$(dirname "$(realpath "$0")")"
echo $SCRIPT_DIR; # /var/tmp/test
```

## Return code

### Example 1

File `script.sh`:
```bash
exit 4
```

Test:
```bash
sh script.sh
echo $? #=> 4
```

### Example 2

File `script.sh`:
```bash
#! /bin/bash

ls some-file.txt
echo "Last command return code: $?"
```

Test:
```bash
sh ./script.sh
ls: cannot access 'some-file.txt': No such file or directory
Last command return code: 2
```
