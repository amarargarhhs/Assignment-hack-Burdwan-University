## 4. Working of UNIX Commands with Syntax

### 1. Terminal (Terminal Emulator)

**Definition:** A terminal is a **text-based interface** (a window program) that allows the user to interact with the operating system by typing commands. In modern UNIX/Linux, the terminal is a **terminal emulator** — a GUI application (e.g., GNOME Terminal, Konsole, xterm, Windows Terminal) that emulates the behaviour of the old physical terminals.

**Working:**
- The terminal displays a **prompt** (e.g., `user@host:~$`) where the user types commands.
- It sends each typed command as **input** to the **Shell** and displays the shell's **output** (stdout) and errors (stderr) on the screen.
- It manages character input/output, history, scrolling, copy/paste, and sometimes multiple tabs.

**Function:** It is the *medium* through which commands reach the shell — the terminal itself does not interpret commands; it only passes them along.

**Syntax:** There is no command syntax for the terminal itself; you simply type commands after the prompt, for example:
```
user@host:~$ ls
```

### 2. Shell

**Definition:** The **shell** is a **command interpreter** — a program that reads commands typed by the user (via the terminal or a script) and instructs the operating system to execute them. Common UNIX shells: `bash` (Bourne Again Shell, the Linux default), `sh`, `csh`, `ksh`, `zsh`.

**Working:**
1. The shell **reads** a command line from the terminal.
2. It **parses** the input, expanding variables (`$HOME`), wildcards (`*`, `?`), command substitution, and redirects (`>`, `<`, `|`).
3. It **locates and executes** the corresponding program (searching the `PATH` directories).
4. It **waits** for the program to finish and then returns the **prompt** for the next command.

**Key features:** command execution; redirection (`>` output, `<` input) and pipes (`|`); scripting (shell scripts `.sh`); job control (background processes with `&`).

**Syntax (generic):**
```
command [options] [arguments]
```

**Example:**
```
user@host:~$ echo "Hello World"
Hello World
```

### 3. cat — Concatenate and Display Files

**Definition:** `cat` is used to **display the contents of a file**, **create** files, and **concatenate** (join) multiple files together.

**Syntax:**
```
cat [options] [file1 file2 ...]
```

**Working:** `cat` reads each named file sequentially and writes its contents to **standard output** (normally the screen). If no file is given, it reads from standard input (keyboard).

**Common options:**
| Option | Function |
|---|---|
| `-n` | Number all output lines |
| `-b` | Number only non-blank lines |
| `-s` | Squeeze repeated blank lines into one |
| `-E` | Show `$` at the end of each line |
| `-A` | Show all hidden characters |

**Examples:**
```
cat file.txt
cat -n file.txt
cat a.txt b.txt > combined.txt
cat > newfile.txt          # create file (type text, then Ctrl+D)
```

### 4. ls — List Directory Contents

**Definition:** `ls` **lists files and directories**. It is one of the most frequently used commands.

**Syntax:**
```
ls [options] [directory/file...]
```

**Working:** `ls` reads the directory structure and displays the names of the files/subdirectories within the specified directory (or the current directory if none is given).

**Common options:**
| Option | Function |
|---|---|
| `-l` | Long format: permissions, owner, size, date |
| `-a` | Show **all** files including hidden files (`.file`) |
| `-h` | Human-readable file sizes (K, M, G) |
| `-R` | List subdirectories recursively |
| `-t` | Sort by modification time (newest first) |
| `-S` | Sort by file size |

**Examples:**
```
ls
ls -l
ls -la
ls -lh /etc
ls -R /home
```

### 5. cd — Change Directory

**Definition:** `cd` (change directory) **moves the user to another directory** — it changes the current working directory of the shell.

**Syntax:**
```
cd [directory]
```

**Working:** `cd` is a **shell built-in** (not a separate program, since it must change the shell's own working directory). It changes location relative to the given **path** (absolute or relative), or to the **home directory** (no arguments, or `~`).

**Special arguments:**
| Argument | Function |
|---|---|
| `cd` or `cd ~` | Go to the user's home directory |
| `cd ..` | Go up one directory level |
| `cd ../..` | Go up two levels |
| `cd -` | Go to the previous directory |
| `cd /` | Go to the root directory |

**Examples:**
```
cd /home/user
cd Documents
cd ..
cd
cd -
```

### 6. date — Display or Set System Date and Time

**Definition:** `date` **displays the current system date and time**, and can also set it (with superuser privileges).

**Syntax:**
```
date [options] [+format]
```

**Working:** `date` reads the system clock and prints the current date and time in a default format, or in a custom format defined by the `+` specifiers.

**Common format specifiers:**
| Specifier | Function |
|---|---|
| `%d` | Day of month (01–31) |
| `%m` | Month (01–12) |
| `%Y` | Year (e.g., 2026) |
| `%H` | Hour (00–23) |
| `%M` | Minute (00–59) |
| `%S` | Second (00–59) |
| `%A` | Full weekday name |

**Examples:**
```
date
date "+%d-%m-%Y"
date "+%H:%M:%S"
date -u
```

### 7. cal — Display a Calendar

**Definition:** `cal` **displays a calendar** of the current month (default), a specific month, or an entire year.

**Syntax:**
```
cal [options] [[month] year]
```

**Working:** `cal` reads the month/year argument (or uses the current date) and prints a text-based calendar for the requested period.

**Common options:**
| Option | Function |
|---|---|
| `-y` | Show the whole current year |
| `-3` | Show previous, current, and next month |
| `-j` | Show days as Julian day-of-year |
| `-m` | Display Monday as the first day |

**Examples:**
```
cal
cal 2026
cal 12 2026
cal -3
cal -y
```

### 8. man — Display Manual (Help) Pages

**Definition:** `man` (manual) **displays the online documentation** (manual pages) for a command, configuration file, or library function.

**Syntax:**
```
man [options] command
```

**Working:** `man` locates the appropriate manual page file and displays it using a pager (usually `less`), which lets you scroll through the documentation. Press `q` to quit, and `/` to search within the page.

**Manual page sections:**
| Section | Content |
|---|---|
| 1 | User commands (e.g., `ls`, `cat`) |
| 5 | Configuration file formats |
| 8 | System administration commands (e.g., `fdisk`) |

**Common options:**
| Option | Function |
|---|---|
| `man -k keyword` | Search for pages matching a keyword (like `apropos`) |
| `man -f command` | Show the section(s) containing the command |
| `man 5 passwd` | Open the manual in section 5 (config file) |

**Examples:**
```
man ls
man -k "calendar"
man 5 crontab
```

### 9. echo — Print Text to the Screen

**Definition:** `echo` **prints its arguments (text) to standard output** — normally the terminal screen.

**Syntax:**
```
echo [options] [string]
```

**Working:** `echo` simply writes the given string (after variable/wildcard expansion by the shell) to the screen. It is a **shell built-in** in most shells, and also exists as an external command (`/bin/echo`).

**Common options:**
| Option | Function |
|---|---|
| `-n` | Do not print a trailing newline |
| `-e` | Interpret escape sequences like `\n` (newline), `\t` (tab) |

**Examples:**
```
echo "Hello World"
echo -n "No newline"
echo -e "Line1\nLine2"
echo $HOME
```

### 10. pwd — Print Working Directory

**Definition:** `pwd` (**print working directory**) **displays the absolute path** of the directory the user is currently in.

**Syntax:**
```
pwd [options]
```

**Working:** `pwd` reads the shell's current working directory and prints its full absolute path (starting from `/`). Like `cd`, it is a shell built-in, but also exists as an external command.

**Common options:**
| Option | Function |
|---|---|
| `-L` | Print the logical path (with symbolic links, default) |
| `-P` | Print the physical path (resolve symbolic links) |

**Examples:**
```
pwd
cd /etc && pwd
```

### 11. mkdir — Make Directory

**Definition:** `mkdir` **creates one or more new directories** (folders).

**Syntax:**
```
mkdir [options] directory_name...
```

**Working:** `mkdir` creates the named directory/directories. By default it will **not** create intermediate (parent) directories that do not exist; the `-p` option overcomes this.

**Common options:**
| Option | Function |
|---|---|
| `-p` | Create parent directories as needed (no error if they exist) |
| `-v` | Verbose — print what is being created |
| `-m mode` | Set permissions (mode) on the new directory |

**Examples:**
```
mkdir newdir
mkdir a b c
mkdir -p dir1/dir2/dir3
mkdir -m 755 secured
mkdir -v projects
```

### 12. rm — Remove Files or Directories

**Definition:** `rm` **deletes files** (and, with options, directories) permanently from the filesystem.

**Syntax:**
```
rm [options] file1 [file2 ...]
```

**Working:** `rm` removes the named files. It is a **permanent deletion** — files are not moved to a trash/recycle bin, so they cannot normally be recovered. `rm` will refuse to delete a non-empty directory unless the `-r` (recursive) option is given.

**Common options:**
| Option | Function |
|---|---|
| `-r` / `-R` | Remove directories and their contents recursively |
| `-f` | Force — ignore nonexistent files, never prompt |
| `-i` | Interactive — ask for confirmation before each deletion |
| `-v` | Verbose — show what is being removed |
| `-d` | Remove an empty directory (like `rmdir`) |

**Examples:**
```
rm file.txt
rm a.txt b.txt
rm -r old_folder
rm -rf temp             # DANGEROUS
rm -i *.log
```

> **Safety note:** `rm -rf` is extremely destructive. As a sysadmin, always double-check the path — a famous mistake is `rm -rf /* ` (the space) which attempts to delete everything. Prefer `rm -i` for important data.

### 13. rmdir — Remove Empty Directories

**Definition:** `rmdir` **deletes empty directories** only. It is the safe counterpart to `rm -d`.

**Syntax:**
```
rmdir [options] directory_name...
```

**Working:** `rmdir` removes the named directory **only if it is empty** (contains no files or subdirectories). If the directory contains anything, `rmdir` fails with an error such as `rmdir: failed to remove 'folder': Directory not empty`. For removing directories *with* contents, use `rm -r`.

**Common options:**
| Option | Function |
|---|---|
| `-p` | Remove parent directories as well |
| `-v` | Verbose — show each directory removed |

**Examples:**
```
rmdir empty_folder
rmdir -p a/b/c
rmdir -v olddir
```

### 14. ps — Report Process Status

**Definition:** `ps` (process status) **displays a snapshot of the currently running processes** along with their PID (Process ID), terminal, CPU/memory usage, and the command that started them.

**Syntax:**
```
ps [options]
```

**Working:** `ps` reads the kernel's process table and prints information about processes. Unlike `top`/`htop`, `ps` shows a **static snapshot** (it does not update continuously).

**Common options:**
| Option | Function |
|---|---|
| `ps` | Show processes for the current terminal (user) |
| `ps -e` | Show **all** processes on the system |
| `ps -ef` | Full listing of all processes (all details) |
| `ps -aux` | Show all processes with detailed info (BSD style) |
| `ps -u user` | Show processes owned by a specific user |
| `ps -p PID` | Show a single process by its PID |

**Important columns in the output:**
| Column | Meaning |
|---|---|
| PID | Process ID (unique number identifying the process) |
| TTY | Terminal the process is attached to (`?` = no terminal) |
| STAT | Process state (e.g., `S` sleeping, `R` running, `Z` zombie) |
| TIME | Total CPU time used by the process |
| CMD | The command that started the process |

**Examples:**
```
ps
ps -ef
ps -aux | more
ps -u root
ps -p 1234
ps -ef | grep firefox
```

### Summary Table — UNIX Commands

| Item | Type | Function | Basic Syntax |
|---|---|---|---|
| Terminal | Interface | Text window to type commands | type commands at prompt |
| Shell | Interpreter | Reads and executes commands | `command [options] [args]` |
| `cat` | Command | Display/concatenate files | `cat [options] [file...]` |
| `ls` | Command | List directory contents | `ls [options] [dir]` |
| `cd` | Command | Change directory | `cd [directory]` |
| `date` | Command | Show/set date and time | `date [+format]` |
| `cal` | Command | Show calendar | `cal [[month] year]` |
| `man` | Command | Display manual pages | `man [options] command` |
| `echo` | Command | Print text to screen | `echo [options] [string]` |
| `pwd` | Command | Print current directory path | `pwd` |
| `mkdir` | Command | Create directories | `mkdir [options] dir...` |
| `rm` | Command | Remove files/directories | `rm [options] file...` |
| `rmdir` | Command | Remove empty directories | `rmdir [options] dir...` |
| `ps` | Command | Show process status | `ps [options]` |

---
