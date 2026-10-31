# Shell Script

A **shell script** is a text file of commands that a shell runs in order. A
script that sets up the environment, runs a program, and then cleans up or
logs results is called a **wrapper**.

Common uses:

- file manipulation
- running programs
- printing text

## Running a script

Give the file execute permission, then run it:

```bash
chmod +x script.sh
./script.sh
```

Or pass it to a shell directly (no execute permission needed):

```bash
bash script.sh
```

## Comments

The shell ignores everything from `#` to the end of the line.

```bash
# This line is a comment
echo "Hello"  # So is this part
```

`#` only starts a comment at the beginning of a word. In `echo "a # b"` or
`$#`, it is not a comment.

## Shebang

The **shebang** (`#!`) on the first line tells the system which interpreter
runs the script. It is followed by the interpreter's path and, optionally, one
argument.

```bash
#!/bin/bash
```

```bash
#!/usr/bin/env bash
```

The second form finds `bash` through `PATH`, so it works even if `bash` is
installed somewhere else. To the shell, the shebang is just a comment.

## Capabilities

### Shortcuts

Turn a long or common command into a short one.

```bash
#!/bin/sh
# ll: list all files in long format
ls -la "$@"
```

`"$@"` passes along any arguments given to the script.

### Batch jobs

Run a series of commands automatically.

```bash
#!/bin/sh
cd ~/project
git pull
make
make test
```

Tools like `cron` can run batch scripts on a schedule.

### Generalization

Use arguments so one script works with different inputs.

```bash
#!/bin/sh
# backup.sh: archive a directory with today's date
src="$1"
tar -czf "backup-$(date +%F).tar.gz" "$src"
```

Run as `./backup.sh ~/documents`.

### Programming

Shells support variables, conditionals (`if`, `case`), loops (`for`, `while`),
and functions.

```bash
#!/bin/bash
for file in *.txt; do
  if [ -s "$file" ]; then
    echo "$file is not empty"
  fi
done
```

## Exit codes

Every command returns an exit code: `0` means success, anything else means
failure. `$?` holds the last command's exit code.

```bash
#!/bin/bash
grep -q "error" log.txt
if [ $? -eq 0 ]; then
  echo "Errors found"
fi
```

Add `set -e` near the top to stop the script at the first failed command.

## Pros and cons

### Pros

- Quick to write for small tasks
- Same syntax as typing commands by hand
- Available on almost every Unix-like system

### Cons

- Slower than compiled programs
- Quoting rules are easy to get wrong
- Not always portable between shells (`sh`, `bash`, `zsh`)
- Hard to maintain as scripts grow

## See also

- [Shell](shell.md)
