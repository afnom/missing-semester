---
layout: lecture
title: "#6: Shell Advanced"
date: 2024-11-11
ready: false
---

<div class="note">
This lesson is contains content written by UoB missing semester volunteers, as well as content adapted from the original MIT Missing Semester lecture notes. This is intended as a supplementary lecture to build off of skills learned in the shell beginner and shell intermediate lectures.
</div>


Hopefully you'll have built up a decent understanding of the linux shell, and the underlying operating system, but there are a few extra tools and tricks to learn that can help you maximise your terminal efficiency and knowledge. This lecture will cover advanced redirection, job control, multiplexing, aliases, advanced scripting, and GNU parallel.


# Advanced redirection

After playing around with pipes and redirection, you may find that some output isn't filtering appropriately through programs such as `grep`, or gets ouputted to terminal even though output is redirected with `>`.
This is due to `>` by default only redirecting STDOUT. 

The default file descriptors and their data streams are labelled as such:

| File descriptor | Name   | Standard stream |
|-----------------|--------|-----------------|
| 0               | STDIN  | Program input   |
| 1               | STDOUT | Program output  |
| 2               | STDERR | Program errors  |

Standard streams 1 and 2 can be redirected by specifying their ID before `>`, for example `cat /non/existent/file 2>error.txt`, which tries to output a non-existent file, and saves the error output in a file called `error.txt`. STDIN (fd 0) isn't often redirected, so you'll mostly be redirecting STDOUT and STDERR.

If you have output for a file that you don't want to save or read, then you can redirect the standard stream to the device file `/dev/null`, which deletes any data sent to it.
As an example, run `echo test` then `echo test >/dev/null` and notice that the error isn't logged in the latter example.
This can also be used with STDERR, for example with `cat /non/existent/file 2>/dev/null`.

Using this, data streams can be split, for example with `python3 -c "print('stdout goes here'); raise Exception('stderr goes here')" >stdout 2>stderr`.
The command might look complicated, but all you need to know is the the `python3` command outputs data to STDOUT and STDERR.
`>stdout.txt` redirects the STDOUT of the program to a file called `stdout.txt`, and `2>stderr` redirects the STDERR of the program to a file called `stderr.txt`.

**Note:** the syntax `&>` can be used to redirect both STDOUT and STDERR, for example using `python3 -c "print('stdout goes here'); raise Exception('stderr goes here')" &>output.txt` to redirect all data to `output.txt`.

You might have noticed that piping to `grep` doesn't filter error messages. This is because `grep` only filters STDOUT. This means that error messages that get sent through STDERR don't get processed by `grep`, and we need to redirect our standard streams to fix this.

If we move the output from STDERR to STDOUT, `grep` will be able to filter it.
We can do this with the syntax `2>&1`. This moves data from STDERR (fd `2`) to STDOUT (fd `1`), using an `&` before `1` to avoid ambiguity between writing to a file named `1`.

Let's practice this by running `python3 -c "raise Exception('This goes to STDERR')"`. This runs the python code `raise Exception('This goes to STDERR')`, which throws an error and outputs context data around it. Trying to filter the output with `python3 -c "raise Exception('This goes to STDERR')" | grep STDERR` doesn't work, since the python error output goes to STDERR, but running `python3 -c "raise Exception('This goes to STDERR')" 2>&1 | grep STDERR` does, since it moves the STDERR stream to STDOUT, which `grep` does filter. As well as this, STDOUT can be redirected to STDERR to properly log errors, such as with `echo 'error here!' >&2`.


# Job control

Sometimes when running a program, some jobs may be run in the background of a shell, allowing processes to be run while commands are executed in the foreground. Running `ps` with no other arguments allows you to see all processes in the current shell. If you don't have any processes currently running in your shell background, your output should look similar to this:

```
  PID TTY          TIME CMD
  344 pts/1    00:00:00 bash
  357 pts/1    00:00:00 ps
```

To run processes in the background, the `&` (ampersand) character can be appended to a command to run it in the background. This turns it into a job. The current job number, as well as system process ID should be printed after running the command, for example `[1] 381`. Try running `sleep 5 &` and `sleep 5` and notice the former allows you to still use the shell while it runs. Running `ps` soon after the backgrounded command should also show the `sleep` process in the shell process list.

**Note**: Even if processes are run backgrounded, output may be sent to the terminal, if you don't want to see the output consider hiding STDOUT and STDERR by using `&>/dev/null` to redirect the program's output.

To background a currently running process, we can suspend and background it. As an example. Let's run `sleep 15`, enter `Ctrl+Z` to suspend the program (there should be an output of the job ID, for example `[1]+`), then run `bg [JOB ID]` (usually 1). Run `ps` again to see the process running in the background. A better way to monitor jobs in the shell is by running `jobs`, which should list any currently running jobs alongside their IDs.

To foreground a job, run `fg [JOB ID]`, feel free to try the above example again using `fg` instead of `bg`. This also works with processes currently backgrounded.

If you want to kill a job, use `kill [JOB ID]` to terminate it.

However, even if a job is backgrounded, closing the terminal will close any processes running from the shell, including jobs running applications started from the shell. To fix this, you can use `disown` to remove processes from the shell's job control list. To test this, let's run `nautilus &`, the default file manager for Ubuntu in the background (if this is unavailable, than any other application with a gui can be used to demonstrate. Close the shell window and notice that `nautilus` (or your chosen application) closes as well. Next, open your shell window again, run `nautilus &`, then type `disown` to clear the shell's job control list. Close the shell window again and see that `nautilus` stays open.

**Note:** `disown` can also be used with a specific process ID, for example `disown 245` to remove a specific process from the shell job control list.


# Multiplexing

Often when using the shell, you might want to see and access multiple shell processes in one tab. To do this, we can use terminal multiplexing. Mutiplexing allows a user to split a terminal into multiple sections, save and restore sessions, and edit theme colouring. One of the most popular terminal multiplexers is `tmux`, and we'll be going through the basics of using `tmux` here.

To start, install the `tmux` package.

**Note:** To always automatically start tmux when you open your terminal, run `echo -e '\nif command -v tmux &> /dev/null && [ -n "$PS1" ] && [[ ! "$TERM" =~ screen ]] && [[ ! "$TERM" =~ tmux ]] && [ -z "$TMUX" ]; then\n  exec tmux\nfi' >> ~/.bashrc`. To disable this, delete the 3 lines starting with `if command -v tmux`.

Operations in `tmux` will start with the prefix `Ctrl-B`, represented by the syntax `C-b` in `tmux` documentation. Although shortcuts can be edited later, we'll be going over the defaults now. `tmux` commands can be outside a tmux session with `tmux` prepended to the command, or in a tmux session after running `C-b :` to access the command line. The following commands will have the `tmux` keyword prepended.


## Session Control

To start, run the `tmux` command to create a new `tmux` session. You should now see a bar on the bottom of screen that says something similar to `[0] 0:bash*`. `tmux` sessions can be listed with `tmux ls`, and existing sessions can be attached to with `tmux attach -a [ID]`. New sessions can be created with another name with `tmux new -s [NAME]`, and to quickly view and switch between sessions, use `C-b s` in a `tmux` session. To kill `tmux` sessions, use `tmux kill-session -t [ID/NAME]`, and to kill every tmux session, use `tmux kill-server`.

## Window + Pane Management

`tmux` sessions by default have 1 window, shown at the bar at the bottom of the screen. To create more, use `C-b c`. To delete windows, use `C-b &`. Windows can be switched between using `tmux select-window -t [ID]`

Each `tmux` window starts with 1 pane, but using commands, we can split this up as we'd like. To split a pane into 2 panes vertically, use `C-b %`, to split a pane into 2 panes horizontally, use `C-b "`. Navigation between these panes can be done with `C-b [ARROW KEY]`, or if `set -g mouse on` is enabled in the command line or `tmux` config file, you can click on panes to activate them. To close a pane, use `C-b x`.


**Note:** You can create a config file in `~/.tmux.conf` or `~/config/tmux.conf` with `tmux` commands such as `set -g mouse on` to save personal preferences and rebind keys.

Here is a useful cheat sheet to refer to while learning `tmux`: https://tmuxcheatsheet.com/


# Aliases

If there's a long command you'll have to use often, you can use an alias instead to run it faster. Aliases work like their name implies, functioning as another name to call a command. The syntax used is `alias [ALIAS_NAME]="[COMMAND]"`, and can be placed in a bash source file to load on every shell, or run in a terminal to work for the current session.

Let's use `sudo apt install` as an example. If you run `alias aptstall="sudo apt install"`, then run `aptstall cmatrix`, you should get prompted for your password, since the shell is running `sudo apt install`. This can be used for any command, and is a quick and easy way to save time when using your terminal.

To save your aliases, one recommendation is to create a `~/.bash_aliases` file, and add `source ~/.bash_aliases` to your `~/.bashrc` file. All of your aliases can be saved here, and will be loaded every time you open a new shell.


# Advanced Shell Scripting

Unlike most other scripting languages, bash uses a variety of special character variables to refer to arguments, error codes, and other relevant variables. Below is a list of some of them. A more comprehensive list can be found [here](https://tldp.org/LDP/abs/html/special-chars.html).
- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` or `Alt+.`

Also note the keyword `shift` that can be used to shift all argument variables down by 1, destroying the previous value of `$1`. This moves the value of `$2` to `$1`, from `$3` to `$2`, and so on. This is useful in cases of iterating through all arguments in a loop.


Commands will often return output using `STDOUT`, errors through `STDERR`, and a return code to report errors in a more script-friendly manner.
The return code or exit status is the way scripts/commands have to communicate how execution went.
A value of 0 usually means everything went OK; anything different from 0 means an error occurred.

Exit codes can be used to conditionally execute commands using `&&` (and operator) and `||` (or operator), both of which are [short-circuiting](https://en.wikipedia.org/wiki/Short-circuit_evaluation) operators. Commands can also be separated within the same line using a semicolon `;`.
The `true` program will always have a 0 return code and the `false` command will always have a 1 return code.
Let's see some examples

```bash
false || echo "Oops, fail"
# Oops, fail

true || echo "Will not be printed"
#

true && echo "Things went well"
# Things went well

false && echo "Will not be printed"
#

true ; echo "This will always run"
# This will always run

false ; echo "This will always run"
# This will always run
```

Another common pattern is wanting to get the output of a command as a variable. This can be done with _command substitution_.
Whenever you place `$( CMD )` it will execute `CMD`, get the output of the command and substitute it in place.
For example, if you do `for file in $(ls)`, the shell will first call `ls` and then iterate over those values.
A lesser known similar feature is _process substitution_, `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file's name. This is useful when commands expect values to be passed by file instead of by STDIN. For example, `diff <(ls foo) <(ls bar)` will show differences between files in dirs  `foo` and `bar`.


Since that was a huge information dump, let's see an example that showcases some of these features. It will iterate through the arguments we provide, `grep` for the string `foobar`, and append it to the file as a comment if it's not found.

```bash
#!/bin/bash

echo "Starting program at $(date)" # Date will be substituted

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

In the comparison we tested whether `$?` was not equal to 0.
Bash implements many comparisons of this sort - you can find a detailed list in the manpage for [`test`](https://www.man7.org/linux/man-pages/man1/test.1.html).
When performing comparisons in bash, try to use double brackets `[[ ]]` in favor of simple brackets `[ ]`. Chances of making mistakes are lower although it won't be portable to `sh`. A more detailed explanation can be found [here](http://mywiki.wooledge.org/BashFAQ/031).

When launching scripts, you will often want to provide arguments that are similar. Bash has ways of making this easier, expanding expressions by carrying out filename expansion. These techniques are often referred to as shell _globbing_.
- Wildcards - Whenever you want to perform some sort of wildcard matching, you can use `?` and `*` to match one or any amount of characters respectively. For instance, given files `foo`, `foo1`, `foo2`, `foo10` and `bar`, the command `rm foo?` will delete `foo1` and `foo2` whereas `rm foo*` will delete all but `bar`.
- Curly braces `{}` - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.

**Note:** Install the `imagemagick` package to use the `convert` command.

```bash
convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# Will expand to
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files


mkdir foo bar
# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```

If a `bash` script runs into an error, it will continue running, which may cause unintended behaviour if an earlier command failure is unhandled, which later commands could rely on. To remedy this, you can use `set -e` at the start of a `bash` script which exits after it receives an error (a non-zero return code).

Writing `bash` scripts can be tricky and unintuitive. There are tools like [shellcheck](https://github.com/koalaman/shellcheck) that will help you find errors in your sh/bash scripts.

Note that scripts need not necessarily be written in bash to be called from the terminal. For instance, here's a simple Python script that outputs its arguments in reversed order:

```python
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

The kernel knows to execute this script with a python interpreter instead of a shell command because we included a [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line at the top of the script.
It is good practice to write shebang lines using the [`env`](https://www.man7.org/linux/man-pages/man1/env.1.html) command that will resolve to wherever the command lives in the system, increasing the portability of your scripts. To resolve the location, `env` will make use of the `PATH` environment variable we introduced in the first lecture.
For this example the shebang line would look like `#!/usr/bin/env python`.

Some differences between shell functions and scripts that you should keep in mind are:
- Functions have to be in the same language as the shell, while scripts can be written in any language. This is why including a shebang for scripts is important.
- Functions are loaded once when their definition is read. Scripts are loaded every time they are executed. This makes functions slightly faster to load, but whenever you change them you will have to reload their definition.
- Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g. change your current directory, whereas scripts can't. Scripts will be passed by value environment variables that have been exported using [`export`](https://www.man7.org/linux/man-pages/man1/export.1p.html)
- As with any programming language, functions are a powerful construct to achieve modularity, code reuse, and clarity of shell code. Often shell scripts will include their own function definitions.


# GNU Parallel

GNU `parallel` is an very useful, customisable tool that uses threads to split multiple commands into threads to speeding up script execution, while preserving the initial command order. `parallel` can be used as a quick and intuitive replacement for `for` loops and `xargs`.

The typical syntax for a `parallel` command looks like `parallel [COMMAND] ::: [INPUT LIST]`, for example, `parallel echo ::: {1..100}`. To repeat a command a certain number of times without passing the number as an argument, the flag `-N0` can be used after `parallel` to pass 0 arguments into the command, letting the command to run the same every time.

As well as using `{1..100}` to expand a list of numbers up to 100, any other list of arguments can be used to iterate over. For example, `parallel ls -la ::: *.txt` lists file metadata for all `.txt` files in the current directory. `parallel` also allows data to be piped into the command, such as `cat /etc/passwd | cut -d ':' -f 1 | parallel id` listing the ID info for all users. Another way of using `parallel` is with the syntax `parallel [COMMAND] :::: [FILE]`, for example with the command `parallel which > paths.txt :::: commands.txt`, which reads all command names in `command.txt` and saves their full paths to `paths.txt`.

One way of stepping up your `parallel` using is by utilising replacement strings. These function as ways to modify arguments given to the command. A couple examples are:

- `{}`   - The argument
- `{#}`  - The index of the argument (starting from 1)
- `{.}`  - The file argument with the extension stripped (e.g: `hello.txt` -> `hello`)
- `{/}`  - The file argument's basename (e.g: `dir/hello.txt` -> `hello.txt`)
- `{//}` - The file argument's directory path (e.g: `dir1/dir2/hello.txt` -> `dir1/dir2`)

These can be used for operations such as converting all jpg images to pngs in parallel, where doing so in a single thread would be much slower, for example (reminder that the `imagemagick` package is required to use `convert`), using the command `parallel convert {} {.}.png ::: *.jpg`. Learning how to use `parallel` effectively can signiificantly increase your terminal efficiency.



# Exercises

To test your learning this session, try the following exercises:

1. Create a bash script that runs the first argument given as a background process that will persist past the terminal's exit, printing the process' name and ID in the terminal with the format `[COMMAND] [PID]`
{% comment %}
```
#!/bin/bash
$1 &
echo $!
disown
```
{% endcomment %}
1. Write a bash function that reads any number of arguments, and writes them to a file `args.txt`, clearing the file contents if it exists, with the syntax `[ARG NUM]: [ARG VALUE]`
{% comment %}
```
args()
{
    echo -n "" > args.txt
    index=1
    for i in $@; do
        echo "$index: $i" >> args.txt
        ((index++))
    done
}
```
{% endcomment %}
1. Build a one-line command that reads website urls from a file `urls.txt`, accesses the websites using multiple threads, and outputs the http status codes to the terminal in the format `[URL] — [CODE]`. Try to hide any progress bars shown when receiving data from websites.
{% comment %}
`parallel "echo -n {} '— '; curl -I {} 2>/dev/null | cut -d ' ' -f 2 | grep -m1 ''" :::: urls.txt`
{% endcomment %}
