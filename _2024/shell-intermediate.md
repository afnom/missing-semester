---
layout: lecture
title: "#2: Intermediate Shell"
date: 2024-10-14
ready: false
---
<div class="note">
<b> Update 08/10/23: </b> The contents below have been adjusted to reflect the Missing Semester content covered at UoB. Check our <a href="https://github.com/afnom/missing-semester/commits/master">git history</a> for a full list of changes.

</div>


In this lecture, we will present some more intermediate concepts around using the shell, including using pipes and redirection, pagers, permissions and groups, basic bash scripts, environment variables, and monitoring your system.

We will also present some of the basics of using bash as a scripting language along with a number of shell tools that cover several of the most common tasks that you will be constantly performing in the command line.


# Pipes and redirection

## Redirection

In the shell, we wish to move data around all the time. This includes moving data between tools on the command line, and moving data in and out of files. So first, let's consider the basics: moving data into files from the shell.

There are many reasons we may want to do this, but they ultimately all boil down to wanting to be able to store some data so it can be recalled later. This could be anything from a note to yourself to a configuration file for a tool you're using.

To write data to a file directly from the shell, we use _redirection_. Redirection simply means that the output of a command has been sent (directed) somewhere else, in this case to a file. Suppose printing 'Hello World':
```bash
echo 'Hello World!'
# Hello World!
```
and we wish to instead write this to the file `hello.txt`. To achieve this we can use the redirection operator `>`, which has syntax `[command] > [filename]`.

`echo` writes to the standard output (stdout) so:
```bash
echo 'Hello World!' > hello.txt
# <no output>
```
Let's check our file. Recall the `cat` command outputs the content of a file to the standard output
```bash
cat hello.txt
# Hello World!
```
Suppose we now wish to write another line to `hello.txt`. If we try
```bash
echo 'Hello from Line 2!' > hello.txt
# <no output>
cat hello.txt
# Hello from Line 2!
```
we find that our original file has been overwritten! The redirection operator `>` always overwrites whatever file is specified. Sometimes this is not desirable, and fortunately the designers of the shell foresaw this issue. Therefore we also have the `>>` operator, which instead _appends_ to the target file rather than overwrites it. Other than that, it functions identically.

```bash
echo 'Hello from Line 2, but actually this time!' >> hello.txt
# <no output>
cat hello.txt
# Hello from Line 2
# Hello from Line 2, but actually this time!
```

As well as this, we can use the `<` operator to redirect the content of a file into a program. Here's an example demonstrating how data can be sent to and from files using redirection operators:

```bash
echo 'hello!' > hello.txt
cat < hello.txt
# hello!
cat < hello.txt > hello2.txt
cat hello2.txt
# hello!
```

Let's now use input event debugging as an example for why we might want to do this. Libinput handles all user input events like keypresses and mouse movements, and if there is a problem with input devices we may wish to review the the Libinput debug logs. We can see these with `sudo libinput debug-events`. However, when moving the mouse or using the touchpad, a huge number of a events are generated and we could not possibly review them all in real time. So instead, we can redirect the output of this command to a file, and then review it afterwards to see if we can spot whatever issue we are pursuing. We will cover reviewing large files in the pagers section below.

## Pipes

Redirection allows changing what files programs write to and read from in a very flexible way (more so than the limited set of functionality described above). However, when we wish to move data between programs, we would like to send the output of one program directly to the input of the next one, rather than writing it to a file and then having the next program read from the same file. This is exactly what the pipe `|` achieves.

Suppose we have a program that outputs some text, and we would like to count how many lines of text there are in its output. First, let us consider just counting how many lines are in some text. We can do this with the `wc` (word count) tool, and we can find out exactly how to configure it to count lines using its manpage (accessible with `man wc`). Now we know that `wc -l` will achieve what we want, we can now consider passing the data from one program into `wc` using a pipe. To do this, we put a pipe between the two commands: `[command 1] | [command2]`.

So if we wish count lines from the output of `ls`, we can:

```bash
ls | wc -l
```
This demonstrates the power of having _composability_. That is, we have the ability to chain multiple programs together with multiple pipes, as the only requirement for this is for a program to read from standard input and to write to standard output.

We will cover the command `grep` (which can be used to filter text) in detail later but know for now that it filters its input against some pattern which we provide.

So if we did
```bash
ls | grep 'hello' | wc -l
```
we now have a program that counts how many lines in a entries in a directory contain the string 'hello'. Perhaps if we are feeling especially fancy we can then redirect the output of `wc` to a file:

```bash
ls | grep 'hello' | wc -l > hellocount.txt
```
incredible.

# Pagers

Reading large volumes of text in the terminal is tricky if it is all just printed onto the screen at once. Perhaps you wish to review a log file that is thousands of lines long, so long in fact that your terminal cannot even scroll back far enough to where the file started being printed! We could use some commands to slice the file into chunks for us, but what we would really like is to be able to review the contents of a file interactively at our own pace. This is exactly what a pager performs - from the name you may guess that it allows you to review text page by page, and you would be correct.

Currently, the most ubiquitous pager has to be `less`. As usual, when we encounter a new tool, we check out what it does by reading the man page, in this case at `man less`. Here, the manual page tells us it is the opposite of `more`, which it also tells us is another program. Not particularly useful. But we can review the manpage of more to find out what it does, aside from the obvious (that the creator of the less thought of an epic pun).

We can see that more displays text content one screen at a time, allowing the user to advance one line with the enter key and use page up / down as expected. Less enhanced this by allowing bidirectional movement using the arrow keys (along with a myriad of other key bindings).

Less is typically used to read from a file, but it can read from standard input as well. As an interactive tool, it also includes help menus and prompts you when a command is not understood. Give it a go with `less somefile`, and press the h key once inside to review the help menu.

As we alluded to earlier, less can do much more than page text. For example, we can search the text below the cursor position with `/[term to search for]`.

# Aside: editing files

We will cover editors (and IDEs) in detail in session #3. However for today it is useful to have a command line file editor, and so for this we will use `nano`. Nano is a small and simple text editor that focusses on usability rather than being extremely powerful like some other editors e.g vim or emacs. To begin editing, we pass the name of the file we wish to edit as the argument, for example `nano somefile`. Nano immediately loads into a TUI (terminal user interface), where instructions for basic commands are onscreen. Note that `^` here refers to the control key, and `M` refers to the meta key (which you probably know as Alt). So when you see `^X` to exit, to trigger this you press Ctrl-X. Other than that it works exactly as you'd expect a text editor to - text appears at your cursor when you type, and you use the arrow keys to move around the file.

# Users, groups and permissions

Linux has a strong permission model where access to files (including programs) and directories is controlled by permissions granted to users and groups. While the concept of a user account is probably quite intuitive, the idea of groups may be new.

Users can belong to many groups. Being in groups grants you all permissions that the group has, and when accessing a resource a user does so with the combined permissions of all of their groups. For example, a University system may have a group for students, which grants access to student resources, and a staff group which grants access to staff resources. While a student or staff member may be in just their respective group, an administrator may be in both the student and staff groups, as this would grant them access to both the student and staff resources.

You can see what groups you're in with the `groups` command. Try it on lab machine to see how CS IT control access.

On a single-user system like a home computer, it's quite common to not be in any groups other than a group named the same as your user (this is created by default, as a user must be in at least one group). This is as there is no need to share resources between users. There are various system administration related groups that your account may be in though, such as `wheel` (provides access to normally restricted commands) or `sudoers` (grants ability to run a command as the root user). Do note though that there are many user accounts and groups that exist for system services. These accounts cannot be logged into and are used for the purposes of isolating programs for security and safety reasons.

You may at this point be wondering exactly how all of these groups and users control access. This is achieved with the permission data present as metadata in the filesystem. Recall the `ls` program used to list files. If we specify the arguments `-l -a` (usually shortened to `-la`), we can see all files including hidden ones (`-a`) listed in a long format with extra detail (`-l`).

```bash
ls -la
##Access        User Group Size Modified     Name
# drwx------ 15 ms   ms    4096 Oct  9 17:48 .
# drwxr-xr-x  4 root root  4096 Oct  7 18:46 ..
# -rw-------  1 ms   ms    5430 Oct  9 09:38 .bash_history
# -rw-r--r--  1 ms   ms      21 May 21 12:56 .bash_logout
# -rw-r--r--  1 ms   ms      57 May 21 12:56 .bash_profile
# -rw-r--r--  1 ms   ms     288 Oct  8 12:09 .bashrc
# drwxr-xr-x 17 ms   ms    4096 Oct  9 09:38 .cache
# drwx------ 14 ms   ms    4096 Oct  9 09:38 .config
# drwxr-xr-x  2 ms   ms    4096 Oct  7 18:47 Desktop
# drwxr-xr-x  2 ms   ms    4096 Oct  8 11:45 Documents
# drwxr-xr-x  2 ms   ms    4096 Oct  8 11:29 Downloads
# -rw-r--r--  1 ms   ms     264 Oct  9 09:38 .gtkrc-2.0
# drwx------  4 ms   ms    4096 Oct  7 18:50 .firefox
# drwxr-xr-x  4 ms   ms    4096 Oct  7 18:47 .local
# drwxr-xr-x  2 ms   ms    4096 Oct  7 18:47 Music
# drwxr-xr-x  2 ms   ms    4096 Oct  8 11:29 Pictures
# drwxr-xr-x  2 ms   ms    4096 Oct  7 18:47 Public
# drwxr-xr-x  2 ms   ms    4096 Oct  8 18:14 .ssh
# drwxr-xr-x  2 ms   ms    4096 Oct  7 18:47 Templates
# drwxr-xr-x  2 ms   ms    4096 Oct  7 18:47 Videos
```
We can now see the access control metadata. The top header naming the columns has been inserted as part of writing this. You may notice the column after access is unlabelled; do not worry about this column as it is not relevant here.

As you can see, each file has one user and one group associated with it. The user is the account that owns the file, and the group is the group assigned to the file. The permissions granted to the user and group are presented in access at the start.

The first character in access is the type of file. For normal files it is blank, and for directories the type is d. There are also other types for special kinds of files, but these will not be covered today. The next nine characters is actually three sets of three. The first three are access for the file owner, the next three are for the group, and the final three are for anyone else. These are generally abbreviated to (u)ser, (g)roup, and (o)ther.

The three characters for each control read, write and execute permissions, hence the characters r, w and x. For files, these operate as you may infer: r allows reading of the file, w allows modifying file content and otherwise manipulating the file, and x allows executing the file. The execute permission is especially important, as it is what separates programs from regular files. Without it there would be no way to tell if a file is meant to be executed or not. For directories, it is a little more complicated, r allows listing directory content, w allows writing files in the directory (provided execute is also present, else it does nothing), and x allows modifying the directory as a structure and using the directory as your current working directory.

For example if a file has permissions `rwxr-x---` it means
<ul>
    <li>The owner can read, write and execute the file</li>
    <li>Any user in the group and read and execute the file, but not write (modify) to it</li>
    <li>Anyone else has no access at all</li>
</ul>

There are also some other values that certain characters can take, but these will not be covered here.

## Changing file permissions

The owner and group of a file can be changed with `chown` (short for change owner). The owner and group can only be changed by the current owner of the file (or the root/superuser).

The access of a file can be changed with `chmod`. Again, this can only be done by the owner of the file, or the root user. The chmod command has versatile syntax allowing permissions to be selectively added and removed rather than just setting a specific value to file.

Both of the above can also be applied recursively to change the permissions of an entire directory structure. Do this with caution! For details on how to use these commands, take a look at their manual pages.

# Package managers

The primary method of installing applications on Linux systems is by using a package manager. Each Linux distribution has its own package manager, but most use one of the following: `apt` (Debian-based), `pacman` (Arch-based), `dnf` (RedHat-based), or `zypper` (SUSE-based). For simplicity's sake, we'll be demonstrating how to use `apt`, as it is the most popular package manager, and the principles should carry on to the other package managers through different command names.

Since package managers edit package files system-wide, root privileges are needed to install, delete, and update package repositories, so most commands will be prefixed by `sudo`.

**Note: Take the same care with using `sudo` now as you always should.**

As as example, let's install `cowsay`:

First, try running `cowsay "Hello!"`. Usually, the `cowsay` command isn't installed, so `bash` should return a `comand not found` error.
Let's start by updating our package repositories with `sudo apt update`. Run `sudo apt install cowsay` and enter your password to install the package. Folloing this, run `cowsay "Hello!"` and notice that the command is now available. To remove `cowsay`, run `sudo apt remove cowsay`.

The following cheat sheet should give you all the commands needed to manage your system packages with `apt`:

| Command             | Function                      | Root privileges required |
|---------------------|-------------------------------|--------------------------|
| `install [PACKAGE]` | Installs a package            | ✓                        |
| `remove [PACKAGE]`  | Removes a package             | ✓                        |
| `update`            | Updates package repositories  | ✓                        |
| `autoremove`        | Removes unneeded dependencies | ✓                        |
| `list`              | Lists installed packages      | ✗                        |
| `search [KEYWORD]`  | Searches available packages   | ✗                        |


# Writing a basic bash script

Throughout Missing Semester so far we have been working with the bash shell to execute various commands. However, bash is more than a shell: it is an entire scripting language. Bash is as powerful as other programming languages, but its syntax is relatively arcane and difficult to use. Coming from other languages such as Python or Java it will be very unfamiliar. It is for this reason we are going to leave the fundamental structures in bash such as if-statements, iteration and function definition / calls to later on.

Even without these bash scripts remain very useful as a way to run many commands together that you might want to run frequently. Here is an example script, which could be entered using nano:
```bash
#!/bin/bash

echo "Blinky time!!"
sudo bash -c 'echo 0 > /sys/class/leds/tpacpi::lid_logo_dot/brightness'
sleep 1
sudo bash -c 'echo 1 > /sys/class/leds/tpacpi::lid_logo_dot/brightness'
sleep 1
sudo bash -c 'echo 0 > /sys/class/leds/tpacpi::lid_logo_dot/brightness'
sleep 1
sudo bash -c 'echo 1 > /sys/class/leds/tpacpi::lid_logo_dot/brightness'
curl --silent https://missingsemester.afnom.net > index.html
echo "Homepage line count"
wc -l index.html
# we could also pipe the output of curl into wc to avoid having to save it to a file
# try modifying the script to do this
```
Obviously this is something you would want to run very frequently, so it's handy to have together in a script rather than having to type out all of these commands every time.

There are a few parts of this script which are new. The first line tells whatever shell that is executing the script to stop and re-execute it with the /bin/bash program, which is the path to the bash shell. This line is needed as it makes the script portable to systems where bash is not the default shell. This can also be used to run other languages. For example, `#!/usr/bin/env python3` can be used to make a Python program directly executable from the shell.

The curl command fetches data from a URL, and then writes it to standard output. It can also do a lot more than this, but we are using it in its simplest configuration here. You can see that the silent parameter is being used: this suppresses progress and status messages from curl. There is another tool, wget, that is also commonly used for fetching URLs. Broadly, they have similar functionality as command line tools.

Suppose the above script has been saved to `missing-semester.sh`. We can then run it with `bash missing-semester.sh`. However, we may wish to run this file as we do with any other program, which we do by specifying the path to it:

```bash
# recall . refers to the current directory
./missing-semester.sh
# bash: ./missing-semester.sh: Permission denied
```
Not exactly what we were hoping for. Let's review the permissions:
```bash
ls -l missing-semester.sh
# -rw-r--r-- 1 ms ms 430 Oct  9 19:00 missing-semester.sh
```
Aha! The file does not have any execute permissions. This is a common problem that occurs when trying to run executables, so if you ever get permission denied on a file you own make sure to double check the permissions. In this case we want execute permissions on the file, so let's add them:
```bash
chmod +x missing-semester.sh # adds execute permission to all users
ls -l missing-semester.sh
# -rwxr-xr-x 1 ms ms 430 Oct  9 19:00 missing-semester.sh
```
Looking better
```bash
./missing-semester.sh
# runs as expected :)
```



# Environment variables

Environment variables define how the shell behaves, and this includes programs that run inside your shell. The `env` command prints all environment variables when executed with no arguments:

```bash
env
# HOME=/home/ms
# LANG=en_GB.UTF-8
# SHELL=/bin/bash
# USER=ms
# PWD=/home/ms/Documents
# XDG_CURRENT_DESKTOP=KDE
# EDITOR=nano
# etc.
```

So for example, if a tool writes a file to the home directory, it needs to know where that is. To resolve its location, it may use the environment variable $HOME. 

One particularly important variable is called PATH. PATH stores where the shell should look to find executable binaries. If you run `cat` in your terminal, the shell has to find out where the actual executable `cat` lies on the system. It does this by searching through PATH, in order from the from the first directory specified, immediately returning when it finds its first match.

For example, PATH may be `/usr/local/sbin:/usr/local/bin:/usr/bin`. If `cat` is ran in the shell, the shell first looks inside `/usr/local/sbin` for the cat executable. If it's in there, then it's done. Else it moves onto the next directory which is `/usr/local/bin`. If it finishes searching all of these and cannot find the file requested, the shell will inform you that the command does not exist. You can use the `which` command to find out where an executable is stored; the which command looks up the location of the executable in the exact same way the shell does:

```bash
which cat
# /usr/bin/cat
```
Let's consider the script (missing-semester.sh) we wrote earlier. Currently, to execute it, we have to call it with its exact path. If we want to be able to call it by name, we have to ensure that is is reachable by the PATH. We could move or copy it to one of the directories already in PATH, but the folders in the PATH above are system-wide, and also managed by the package manager. It is therefore not recommended to store your own scripts in them. Instead, since this executable is just for the current user (us), we'll add a directory to our own PATH.

It is custom to put user executables in `~/.local/bin` (where ~ means the home directory), but for this demonstration we will make our own folder in the home directory called `executables`. Then we can add it to the PATH, and move our script there.

```bash
mv missing-semester.sh missing-semester # let's rename our script first as programs being run in the shell generally avoid file extensions in their names (this is just for neatness)
missing-semester # try it, expecting not found as it's not in PATH
# bash: missing-semester: command not found
mkdir executables
PATH="$PATH:/home/ms/executables" # note the use of double quotes here. this enables variable expansion, so $PATH gets expanded into the actual value of $PATH
mv missing-semester executables/ # move to the executables directory
missing-semester
# and we're running!
```

With this setup, every executable placed inside the executables folder will be available to us in this shell.

New environment variables can be set using `[NAME]=[VALUE]`, where the variable can be accessed with `$[NAME]`.

Importantly, do note that this change isn't permanent and only applies to the particular shell you have changed the environment variable in. If you wished to apply this change to every shell, you could modify the `~/.bashrc` file, as this file is executed as part of the start-up process for all shells.

In the computer labs at UoB, you may find that you are asked to use the `module` command to make some program available. This works exactly like our PATH change above. The software you're loading with `module` is actually present on the computer all the time, but it's not found by your shell as it's not in the PATH. Once you run the `module load` command, it gets inserted into your PATH so your shell can find it.

Broadly, there is always a reason for the behaviour you see from a Linux computer, and tools like `env` grant you the power to look under the hood and see how it all fits together. Try running `env` the next time you're on a lab machine!

# Monitoring your system

One key part of maintaining a system is being aware of what programs are running on it. This is useful to know just to understand what is happening on your system, but it's especially useful if there are programs hogging resources or otherwise behaving badly that you might want to act on.

For example, your system might be bogged down by an application thrashing the disk or an application using a lot of memory, and you might want to know what program is causing the slowdown. To do this you can use the the `top` program. top gives you a list of all of the processes running, and by default sorts them by CPU usage. It provides information about the processes, such as their priority, total CPU time and memory usage. It also provides an overview of the total load of the system, what CPU utilisation is like and how much memory is used/available in total. Additionally, you can take actions on processes inside of top such as terminating them or changing their priority. Pressing the h key while inside top will display a brief help message showing what functionality is available.

While top is an interactive tool, sometimes we also wish to simply list processes. We can do this with the `ps` tool. ps supports many flags to control what fields are output, and a typical invocation to list all processes with full information is `ps faux`. `ps fauxw` can be used instead to do the same, but print the full command string of processes as well. As a typical system has a lot of processes, this outputs many lines. Subprocesses are shown branching off their parent processes. If we are looking for a specific process, we can pipe the output of ps into `grep` to filter it. Suppose we're looking running instances of the program konsole (the terminal being used to demonstrate this):

```bash
# for reference, the normal ps header
# USER PID    %CPU %MEM VSZ     RSS    TTY STAT START TIME COMMAND

ps -aux | grep konsole
# ms   511185 0.1  0.5  1085812 194112 ?   Sl   21:15 0:11 /usr/bin/konsole
# ms   512281 0.0  0.5  1085928 194152 ?   Sl   21:28 0:00 /usr/bin/konsole
# ms   516264 0.4  0.5  1086616 192632 ?   Sl   22:52 0:01 /usr/bin/konsole
```

and we can immediately see the three terminals open as this guide is written.


# Shell Scripting

So far we have seen how to execute commands in the shell and pipe them together.
However, in many scenarios you will want to perform a series of commands and make use of control flow expressions like conditionals or loops.

Shell scripts are the next step in complexity.
Most shells have their own scripting language with variables, control flow and its own syntax.
What makes shell scripting different from other scripting programming language is that it is optimized for performing shell-related tasks.
Thus, creating command pipelines, saving results into files, and reading from standard input are primitives in shell scripting, which makes it easier to use than general purpose scripting languages.
For this section we will focus on bash scripting since it is the most common.

To assign variables in bash, use the syntax `foo=bar` and access the value of the variable with `$foo`.
Note that `foo = bar` will not work since it is interpreted as calling the `foo` program with arguments `=` and `bar`.
In general, in shell scripts the space character will perform argument splitting. This behavior can be confusing to use at first, so always check for that.

Strings in bash can be defined with `'` and `"` delimiters, but they are not equivalent.
Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimited strings will.

```bash
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```

As with most programming languages, bash supports control flow techniques including `if`, `case`, `while` and `for`.
Similarly, `bash` has functions that take arguments and can operate with them. Here is an example of a function that creates a directory and `cd`s into it.


```bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```

Here `$1` is the first argument to the script/function. The syntax `$n` can be used to refer to arguments to a bash script/function, where n is the argument number, and `$0` refers to the name of the script being run.


# Shell Tools

## Finding how to use commands

At this point, you might be wondering how to find the flags for the commands in the aliasing section such as `ls -l`, `mv -i` and `mkdir -p`.
More generally, given a command, how do you go about finding out what it does and its different options?
You could always start googling, but since UNIX predates StackOverflow, there are built-in ways of getting this information.

As we saw in the shell lecture, the first-order approach is to call said command with the `-h` or `--help` flags. A more detailed approach is to use the `man` command.
Short for manual, [`man`](https://www.man7.org/linux/man-pages/man1/man.1.html) provides a manual page (called manpage) for a command you specify.
For example, `man rm` will output the behavior of the `rm` command along with the flags that it takes, including the `-i` flag we showed earlier.
In fact, what I have been linking so far for every command is the online version of the Linux manpages for the commands.
Even non-native commands that you install will have manpage entries if the developer wrote them and included them as part of the installation process.
For interactive tools such as the ones based on ncurses, help for the commands can often be accessed within the program using the `:help` command or typing `?`.

Sometimes manpages can provide overly detailed descriptions of the commands, making it hard to decipher what flags/syntax to use for common use cases.
[TLDR pages](https://tldr.sh/) are a nifty complementary solution that focuses on giving example use cases of a command so you can quickly figure out which options to use.
For instance, I find myself referring back to the tldr pages for [`tar`](https://tldr.inbrowser.app/pages/common/tar) and [`ffmpeg`](https://tldr.inbrowser.app/pages/common/ffmpeg) way more often than the manpages.
`tldr` can also be installed using your default package manager and run in the terminal, for example with: `tldr ls`, which is very useful for quick reminders on command syntax and usage.


## Finding files

One of the most common repetitive tasks that every programmer faces is finding files or directories.
All UNIX-like systems come packaged with [`find`](https://www.man7.org/linux/man-pages/man1/find.1.html), a great shell tool to find files. `find` will recursively search for files matching some criteria. Some examples:

```bash
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```

Beyond listing files, find can also perform actions over files that match your query.
This property can be incredibly helpful to simplify what could be fairly monotonous tasks.

```bash
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

**Note:** `{}` here represents the name of the file found for each `-exec` command, so if a file `file.txt` is found, `-exec rm {} \;` would run `rm file.txt`.

Despite `find`'s ubiquitousness, its syntax can sometimes be tricky to remember.
For instance, to simply find files that match some pattern `PATTERN` you have to execute `find -name '*PATTERN*'` (or `-iname` if you want the pattern matching to be case insensitive).
You could start building aliases for those scenarios, but part of the shell philosophy is that it is good to explore alternatives.
Remember, one of the best properties of the shell is that you are just calling programs, so you can find (or even write yourself) replacements for some.
For instance, [`fd`](https://github.com/sharkdp/fd) is a simple, fast, and user-friendly alternative to `find`.
It offers some nice defaults like colorized output, default regex matching, and Unicode support. It also has, in my opinion, a more intuitive syntax.
For example, the syntax to find a pattern `PATTERN` is `fd PATTERN`.

Most would agree that `find` and `fd` are good, but some of you might be wondering about the efficiency of looking for files every time versus compiling some sort of index or database for quickly searching.
That is what [`locate`](https://www.man7.org/linux/man-pages/man1/locate.1.html) is for.
`locate` uses a database that is updated using [`updatedb`](https://www.man7.org/linux/man-pages/man1/updatedb.1.html).
In most systems, `updatedb` is updated daily via [`cron`](https://www.man7.org/linux/man-pages/man8/cron.8.html).
Therefore one trade-off between the two is speed vs freshness.
Moreover `find` and similar tools can also find files using attributes such as file size, modification time, or file permissions, while `locate` just uses the file name.
A more in-depth comparison can be found [here](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other).

## Finding code

Finding files by name is useful, but quite often you want to search based on file *content*. 
A common scenario is wanting to search for all files that contain some pattern, along with where in those files said pattern occurs.
To achieve this, most UNIX-like systems provide [`grep`](https://www.man7.org/linux/man-pages/man1/grep.1.html), a generic tool for matching patterns from the input text.
`grep` is an incredibly valuable shell tool that we will cover in greater detail during the data wrangling lecture.

For now, know that `grep` has many flags that make it a very versatile tool.
Some commonly used flags include `grep -i` for case **i**nsensitive matching, `-C` for getting **C**ontext around the matching line and `-v` for in**v**erting the match, i.e. print all lines that do **not** match the pattern. For example, `grep -C 5` will print 5 lines before and after the match.
When it comes to quickly searching through many files, you want to use `-R` since it will **R**ecursively go into directories and look for files for the matching string.

But `grep -R` can be improved in many ways, such as ignoring `.git` folders, using multi CPU support, &c.
Many `grep` alternatives have been developed, including [ack](https://github.com/beyondgrep/ack3), [ag](https://github.com/ggreer/the_silver_searcher) and [rg](https://github.com/BurntSushi/ripgrep).
All of them are fantastic and pretty much provide the same functionality.
For now I am sticking with ripgrep (`rg`), given how fast and intuitive it is. Some examples:
```bash
# Find all python files where I used the requests library
rg -t py 'import requests'
# Find all files (including hidden files) without a shebang line
rg -u --files-without-match "^#\!"
# Find all matches of foo and print the following 5 lines
rg foo -A 5
# Print statistics of matches (# of matched lines and files )
rg --stats PATTERN
```

Note that as with `find`/`fd`, it is important that you know that these problems can be quickly solved using one of these tools, while the specific tools you use are not as important.

## Finding shell commands

So far we have seen how to find files and code, but as you start spending more time in the shell, you may want to find specific commands you typed at some point.
The first thing to know is that typing the up arrow will give you back your last command, and if you keep pressing it you will slowly go through your shell history.

The `history` command will let you access your shell history programmatically.
It will print your shell history to the standard output.
If we want to search there we can pipe that output to `grep` and search for patterns.
`history | grep find` will print commands that contain the substring "find".

In most shells, you can make use of `Ctrl+R` to perform backwards search through your history.
After pressing `Ctrl+R`, you can type a substring you want to match for commands in your history.
As you keep pressing it, you will cycle through the matches in your history.
This can also be enabled with the UP/DOWN arrows in [zsh](https://github.com/zsh-users/zsh-history-substring-search).
A nice addition on top of `Ctrl+R` comes with using [fzf](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r) bindings.
`fzf` is a general-purpose fuzzy finder that can be used with many commands.
Here it is used to fuzzily match through your history and present results in a convenient and visually pleasing manner.

Another cool history-related trick I really enjoy is **history-based autosuggestions**.
First introduced by the [fish](https://fishshell.com/) shell, this feature dynamically autocompletes your current shell command with the most recent command that you typed that shares a common prefix with it.
It can be enabled in [zsh](https://github.com/zsh-users/zsh-autosuggestions) and it is a great quality of life trick for your shell.

You can modify your shell's history behavior, like preventing commands with a leading space from being included. This comes in handy when you are typing commands with passwords or other bits of sensitive information.
To do this, add `HISTCONTROL=ignorespace` to your `.bashrc` or `setopt HIST_IGNORE_SPACE` to your `.zshrc`.
If you make the mistake of not adding the leading space, you can always manually remove the entry by editing your `.bash_history` or `.zsh_history`.

## Directory Navigation

So far, we have assumed that you are already where you need to be to perform these actions. But how do you go about quickly navigating directories?
There are many simple ways that you could do this, such as writing shell aliases or creating symlinks with [ln -s](https://www.man7.org/linux/man-pages/man1/ln.1.html), but the truth is that developers have figured out quite clever and sophisticated solutions by now.

As with the theme of this course, you often want to optimize for the common case.
Finding frequent and/or recent files and directories can be done through tools like [`fasd`](https://github.com/clvv/fasd) and [`autojump`](https://github.com/wting/autojump).
Fasd ranks files and directories by [_frecency_](https://web.archive.org/web/20210421120120/https://developer.mozilla.org/en-US/docs/Mozilla/Tech/Places/Frecency_algorithm), that is, by both _frequency_ and _recency_.
By default, `fasd` adds a `z` command that you can use to quickly `cd` using a substring of a _frecent_ directory. For example, if you often go to `/home/user/files/cool_project` you can simply use `z cool` to jump there. Using autojump, this same change of directory could be accomplished using `j cool`.

More complex tools exist to quickly get an overview of a directory structure: [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) or even full fledged file managers like [`nnn`](https://github.com/jarun/nnn) or [`ranger`](https://github.com/ranger/ranger).

# Exercises

1. Read [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html) and write an `ls` command that lists files in the following manner

    - Includes all files, including hidden files
    - Sizes are listed in human readable format (e.g. 454M instead of 454279954)
    - Files are ordered by recency
    - Output is colorized

    A sample output would look like this

    ```
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

{% comment %}
ls -lath --color=auto
{% endcomment %}

1. Write bash functions  `marco` and `polo` that do the following.
Whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`.
For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.

{% comment %}
marco() {
    export MARCO=$(pwd)
}

polo() {
    cd "$MARCO"
}
{% endcomment %}

1. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run.
Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end.
Bonus points if you can also report how many runs it took for the script to fail.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
       echo "Something went wrong"
       echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

{% comment %}
#!/usr/bin/env bash

count=0
until [[ "$?" -ne 0 ]];
do
  count=$((count+1))
  ./random.sh &> out.txt
done

echo "found error after $count runs"
cat out.txt
{% endcomment %}

1. As we covered in the lecture `find`'s `-exec` can be very powerful for performing operations over the files we are searching for.
However, what if we want to do something with **all** the files, like creating a zip file?
As you have seen so far commands will take input from both arguments and STDIN.
When piping commands, we are connecting STDOUT to STDIN, but some commands like `tar` take inputs from arguments.
To bridge this disconnect there's the [`xargs`](https://www.man7.org/linux/man-pages/man1/xargs.1.html) command which will execute a command using STDIN as arguments.
For example `ls | xargs rm` will delete the files in the current directory.

    Your task is to write a command that recursively finds all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces (hint: check `-d` flag for `xargs`).
    {% comment %}
    find . -type f -name "*.html" | xargs -d '\n'  tar -cvzf archive.tar.gz
    {% endcomment %}

    If you're on macOS, note that the default BSD `find` is different from the one included in [GNU coreutils](https://en.wikipedia.org/wiki/List_of_GNU_Core_Utilities_commands). You can use `-print0` on `find` and the `-0` flag on `xargs`. As a macOS user, you should be aware that command-line utilities shipped with macOS may differ from the GNU counterparts; you can install the GNU versions if you like by [using brew](https://formulae.brew.sh/formula/coreutils).

1. (Advanced) Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?
