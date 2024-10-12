Here's a summarized version of the "Basic Shell Skills" chapter in markdown format for a `README.md` file, with all relevant commands included:

```md
# **RHCSA (EX200) TRAINING**

## **Basic Shell Skills**

The shell is the default working environment for a Linux administrator. Bash is the most common shell. The shell allows users and administrators to execute commands, manipulate files, and manage processes.

### **Understanding Commands**

A command typically consists of:
- The **command** itself (e.g., `ls` to list files).
- **Options** modify the behavior of the command (e.g., `ls -l` shows long listing).
- **Arguments** target specific files or directories (e.g., `ls -l /etc`).

Command example:
```bash
ls -l /etc
```

This command has two arguments:
- `-l`: An option that modifies the command.
- `/etc`: The target where the command operates.

### **Executing Commands**

There are three types of commands:
- **Aliases**: Custom shortcuts for commands. Example:
  ```bash
  alias newcommand='oldcommand'
  ```
  Check existing aliases:
  ```bash
  alias
  ```
- **Internal commands**: Built into the shell (e.g., `cd`, `echo`).
- **External commands**: Executable files on disk (e.g., `ls`, `grep`).

Find command types:
```bash
type commandname
which commandname
```

Example:
```bash
type time
which ls
```

### **$PATH Variable**

The shell uses the `$PATH` variable to locate executables. To view the current `$PATH`:
```bash
echo $PATH
```

To run a command from the current directory:
```bash
./scriptname
```

### **I/O Redirection**

Commands have standard input (STDIN), standard output (STDOUT), and standard error (STDERR). Redirect output or errors using:
- Redirect output: 
  ```bash
  command > file
  ```
- Redirect error:
  ```bash
  command 2> errorfile
  ```

Example:
```bash
ls > outputfile
ls wrongfile 2> errorfile
```

To discard output:
```bash
command > /dev/null
```

### **Using Pipes**

Pipes (`|`) connect the output of one command to the input of another. Example:
```bash
ls | less
```

### **Bash History**

Bash saves command history for easy repetition. Useful commands:
- View history:
  ```bash
  history
  ```
- Repeat a command:
  ```bash
  !commandnumber
  !command
  ```
- Search through history:
  ```bash
  Ctrl + r
  ```

To clear history:
```bash
history -c
```

### **Bash Completion**

Bash automatically completes commands, variables, and filenames. Press the `Tab` key after typing part of a command for auto-completion.

```bash
ls <Tab>
```

### **Exercises**

#### **Exercise 1: Using Internal and External Commands**
1. Authenticate on the server and open a terminal.
2. Run the following commands:
   ```bash
   time ls
   which time
   type time
   echo $PATH
   /usr/bin/time ls
   ```

#### **Exercise 2: Using I/O Redirection and Pipes**
1. Run the following:
   ```bash
   ls > /dev/null
   ls wrongfile 2> /dev/null
   ls /etc 2> /dev/null > output
   cat output
   echo hello > output
   ls >> output
   ls -R / | less
   ```

#### **Exercise 3: Working with History**
1. Run the following:
   ```bash
   history
   Ctrl + r (search for commands)
   history | grep cat
   !number
   history -c
   ```

#### **Exercise 4: Bash Completion**
1. Test Bash completion with commands:
   ```bash
   ls <Tab>
   ```
```
