# Shell

A **shell** is a program that lets you use the services of an operating system
(OS). It sits between you and the OS: it reads your commands, asks the OS to
carry them out, and shows you the results.

```text
Request:  User -> Shell -> OS (kernel) -> Hardware
Response: User <- Shell <- OS (kernel) <- Hardware
```

A shell is not part of the kernel. It is an ordinary program that uses the
kernel's API (system calls), like any other application.

Common uses:

- file management
- process management
- batch processing (scripting)
- system monitoring
- system configuration

## Types of shells

- **Command-line shells (CLI):** bash, zsh, fish, PowerShell, cmd.exe
- **Graphical shells (GUI):** Windows Explorer, GNOME Shell, macOS Finder

| Shell      | Notes                                          |
| ---------- | ---------------------------------------------  |
| sh         | The original Unix shell; the portable baseline |
| bash       | Default on most Linux distributions            |
| zsh        | Default on macOS; bash-compatible with extras  |
| fish       | Friendly defaults; not POSIX-compatible        |
| PowerShell | Windows; works with objects instead of text    |

## Shell vs. terminal

- The **terminal** is the window that handles text input and output.
- The **shell** is the program running inside it that interprets what you type.

## How a shell runs a command

1. Show a prompt and read a line of input.
2. Expand variables, wildcards, and aliases.
3. Run a built-in command, or start an external program as a new process.
4. Wait for it to finish, then show the prompt again.

## Built-in vs. external commands

- **Built-in** commands run inside the shell itself, such as `cd`, `export`,
  and `alias`. They must be built in because they change the shell's own state.
- **External** commands are separate programs, such as `ls` and `grep`.

Use `type` to check:

```bash
type cd   # cd is a shell builtin
type ls   # ls is /usr/bin/ls
```

## PATH

`PATH` is a list of directories the shell searches when you type a command
name.

```bash
echo $PATH
# /usr/local/bin:/usr/bin:/bin
```

The current directory is usually not in `PATH`. That is why you run a local
script as `./script.sh` instead of `script.sh`.

## Environment variables

Variables store values the shell and programs can use.

```bash
echo $HOME          # print a variable
name="Ada"          # set a shell variable
export EDITOR=vim   # make it available to child processes
```

## Pipes and redirection

A **pipe** (`|`) sends one command's output into another command's input.
**Redirection** sends input or output to and from files.

| Symbol | Meaning                            |
| ------ | ---------------------------------- |
| `\|`   | Pipe output to another command     |
| `>`    | Write output to a file (overwrite) |
| `>>`   | Append output to a file            |
| `<`    | Read input from a file             |
| `2>`   | Write errors to a file             |

```bash
ls | grep txt > results.txt
```

## Wildcards

The shell expands wildcards into matching file names before running the
command.

| Pattern | Matches                       |
| ------- | ----------------------------- |
| `*`     | Any number of characters      |
| `?`     | Exactly one character         |
| `[abc]` | One of the listed characters  |

```bash
ls *.txt      # all .txt files
ls file?.md   # file1.md, fileA.md, ...
```

## Configuration files

Shells read startup files where you can set aliases, variables, and your
prompt.

- bash: `~/.bashrc`
- zsh: `~/.zshrc`

```bash
alias ll='ls -la'
export PATH="$HOME/bin:$PATH"
```

## Interactive features

- **History:** Up arrow for previous commands; `Ctrl+R` to search them.
- **Tab completion:** Press `Tab` to complete commands and file names.
- **Job control:** `&` runs a command in the background; `Ctrl+Z` pauses it;
  `fg` and `bg` resume it.

## See also

- [Shell Script](shell-script.md)
