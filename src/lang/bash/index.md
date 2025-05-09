# Bash

```bash
#!/bin/bash
# above is shebang, the path to the interpreter

echo "Hello, World!"
```

## Variables & Arguments

```bash
# declare a variable
name="Saplyn"

# access the variable
echo "I'm $name!"

$? # exit status of the last command
$$ # process ID of the current script
$! # process ID of the last background command

$0        # name of the script
$1 ... $9 # arguments passed to the script
$#        # number of arguments passed to the script
$*        # all arguments passed to the script

${#name} # length of the variable
${#1}    # length of the first argument
```

## Command & Substitution

```bash
# execute a command
echo "Hello, World!"

# command substitution
current_date=$(date)
echo "Today is $current_date. I'm $(whoami)"
```

## Control Flow

### Branching

```bash
# if-else
if true; then
    echo "It's true"
else
    echo "It's false"
fi

# case
case $1 in
    start)
        echo "Starting..."
        ;;
    stop)
        echo "Stopping..."
        ;;
    *)
        echo "Unknown command"
        ;;
esac

# number comparison
[$x -eq $y] # equal
[$x -ne $y] # not equal
[$x -gt $y] # greater than
[$x -lt $y] # less than
[$x -ge $y] # greater than or equal
[$x -le $y] # less than or equal

# string comparison
[$s =  $t] # equal
[$s != $t] # not equal
[$s <  $t] # less than
[$s >  $t] # greater than
[-z $s]    # empty
[-n $s]    # not empty

# file assertion
[-e $f] # exists
[-f $f] # is regular file
[-d $f] # is directory
[-r $f] # readable
[-w $f] # writable
[-x $f] # executable
[-s $f] # not empty
[-u $f] # SUID is set

# logical operators
[$cond -a $cond]     # and
[$cond -o $cond]     # or
! [$cond]            # not
[$cond1] && [$cond2] # short-circuit and
[$cond1] || [$cond2] # short-circuit or
```

### Looping

```bash
# for-in
for i in 1 2 3; do
    echo $i
done
for i in {1..3}; do
    echo $i
done

# c-style for
for ((i=0; i<3; i++)); do
    echo $i
done

# while
i=0
while [ $i -le 5 ]; do
    echo "Executing \`while\` loop: $i"
    i=$((i + 1))
done

# until
i=0
until [ $i -ge 5 ]; do
    echo "Executing \`until\` loop: $i"
    i=$((i + 1))
done
```

```bash
# break
for i in {1..10}; do
    if [ "$i" -eq 5 ]; then
        break
    fi
    echo "Loop: $i"
done

# break multiple loops
for i in {1..10}; do
    for j in {1..2}; do
        if [ "$i" -eq 5 ]; then
            break 2
        fi
        echo "($i, $j)"
    done
done

# continue
for i in {1..10}; do
    if [ "$i" -eq 5 ]; then
        continue
    fi
    echo "Loop: $i"
done

# continue multiple loops
for i in {1..5}; do
    for j in {1..2}; do
        if [ "$i" -eq 3 ]; then
            continue 2
        fi
        echo -n "($i, $j) "
    done
    echo "($i)"
done
```

### Exit

```bash
# exit
exit

# exit with status
exit 0 # success
exit 1 # failure (non-zero status)
```

## Functions

```bash
# declaration
function greet() {
    echo "Hello, World!"
}
greet # call

# with arguments
function greet() {
    echo "Hello, $1!"
    echo "Hello, $2, too!"
}
greet "World" "Saplyn" # call
```
