Here's the corrected version of the "Basic Shell Skills" chapter in markdown format for a `README.md` file, with all relevant commands included:

```md
# **RHCSA (EX200) TRAINING**

## **Basic Shell Skills**

The shell is the default working environment for a Linux administrator. Bash is the most common shell. The shell allows users and administrators to execute commands, manipulate files, and manage processes.

### **Understanding Commands**

A command typically consists of:
- The **command** itself (e.g., `ls` to list files).
- **Options** that modify the behavior of the command (e.g., `ls -l` shows long listing).
- **Arguments** that target specific files or directories (e.g., `ls -l /etc`).

Command example:
```bash
ls -l /etc
```

This command has two parts:
- `-l`: An option that modifies the command.
- `/etc`: The target directory where the command operates.

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

## **Editing Files with `vim`**

Linux system administration often requires editing configuration files, and the most widely available text editor is `vi` or its improved version, `vim`. While `vim` offers many enhancements, such as syntax highlighting, its basic commands are identical to `vi`.

### **`vim` Modes**
`vim` operates in two essential modes:
- **Command Mode**: For issuing commands.
- **Input Mode**: For editing the file's contents.

Switch between these modes as needed for your tasks.

### **Essential `vim` Commands**
The following commands are essential when using `vim`:
- **i**: Enter Input Mode.
- **Esc**: Exit Input Mode and return to Command Mode.
- **:w**: Write (save) the file.
- **:q**: Quit `vim`.
- **dd**: Delete the current line.
- **u**: Undo the last action.
- **:%s/old/new/g**: Replace `old` with `new` throughout the file.

To practice these commands, follow **Exercise 2-5** where you will create and edit a file using `vim`.

## **Understanding the Shell Environment**

A shell environment consists of variables that define the user environment. Examples include:
- `$PATH`: Defines directories to search for executable files.
- `$LANG`: Specifies language settings.

To view the current environment variables, use the `env` command. To modify a variable temporarily, you can assign it a new value, e.g., `LANG=es_ES.UTF-8`.

### **Exercise: Shell Environment Practice**
1. Open a shell and type `echo $LANG` to display your current language setting.
2. Temporarily change the language by typing `LANG=es_ES.UTF-8`.
3. Open `.bashrc` and add a new variable (e.g., `COLOR=red`).

## **Finding Help on Linux**

Hundreds of commands are available on Linux, but it's impractical to memorize them all. Instead, focus on how to find the help you need:
- **--help**: Display a usage summary for a command.
- **man**: Show detailed manual pages for commands.
- **apropos** or `man -k`: Search for commands based on a keyword.
- **info**: Provides hierarchical documentation for commands.

### **Exercise: Using `man` Pages**
1. Search for `man -k partition` to find commands related to partitions.
2. Use `grep` to filter results by specific sections, e.g., `man -k partition | grep 8`.

### **Tip**
To ensure up-to-date `man` pages, periodically run `mandb` to update the `mandb` database.
```