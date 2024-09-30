---
layout: lecture
title: "#6: Shell Advanced"
date: 2024-11-11
ready: false
---

<div class="note">
This lesson is a UoB original and has been completely written from scratch by @bluka479. This is intended as a supplementary lecture to build off of skills learned in the shell beginner and shell intermediate lectures.
</div>


Hopefully you'll have built up a decent understanding of the linux shell, and the underlying operating system, but there are a few extra tools and tricks to learn that can help you maximise your terminal efficiency and knowledge. This lecture will cover standard streams, backgrounding processes, multiplexing, and aliases.


# Advanced redirection

After playing around with pipes and redirection, you may find that some out isn't filtering appropriately through programs such as `grep`, or gets ouputted to terminal even though output is redirected with `>`.
This is due to `>` by default only redirecting STDOUT. 

The default file descriptors and their data streams are labelled as such:

| File descriptor | Name   | Standard stream |
|-----------------|--------|-----------------|
| 0               | STDIN  | Program input   |
| 1               | STDOUT | Program output  |
| 2               | STDERR | Program errors  |

Standard streams can be redirected by specifying their id before `>`, for example `cat /non/existent/file 2>error.txt`, which tries to output a non-existent file, and saves the error output in a file called `error.txt`. STDIN (fd 0) isn't often redirected, so you'll mostly be redirecting STDOUT and STDERR.

If you have output for a file that you don't want to save or read, then you can redirect the standard stream to the device file `/dev/null`, which deletes any data sent to it.
As an example, run `echo test` then `echo test >/dev/null` and notice that the error isn't logged in the latter example.
This can also be used with STDERR, for example with `cat /non/existent/file 2>/dev/null`.

Using this, data streams can be split, for example with `python3 -c "print('stdout goes here'); raise Exception('stderr goes here')" >stdout 2>stderr`.
The command might look complicated, but all you need to know is the the `python3` command outputs data to STDOUT and STDERR.
`>stdout.txt` redirects the STDOUT of the program to a file called `stdout.txt`, and `2>stderr` redirects the STDERR of the program to a file called `stderr.txt`.

**Note:** the syntax `&>` can be used to redirect both STDOUT and STDERR, for example using `python3 -c "print('stdout goes here'); raise Exception('stderr goes here')" &>output.txt` to redirect all data to `output.txt`

You might have noticed that piping to `grep` doesn't filter error messages. This is because `grep` only filters STDOUT. This means that error messages that get sent through STDERR don't get processed by `grep`, and we need to redirect our standard streams to fix this.

If we move the output from STDERR to STDOUT, `grep` will be able to filter it.
We can do this with the syntax `2>&1`. This moves data from STDERR (fd `2`) to STDOUT (fd `1`), using an `&` before `1` to avoid ambiguity between writing to a file named `1`

Let's practice this by running `python3 -c "raise Exception('This goes to STDERR')"`. This runs the python code `raise Exception('This goes to STDERR')`, which throws an error and outputs context data around it. Trying to filter the output with `python3 -c "raise Exception('This goes to STDERR')" | grep STDERR` doesn't work, since the python error output goes to STDERR, but running `python3 -c "raise Exception('This goes to STDERR')" 2>&1 | grep STDERR` does, since it moves the STDERR stream to STDOUT, which `grep` does filter.


# Background processes

Sometimes when running a program, some processes or subprocesses may be run in the background of a shell, allowing processes to be run while commands are executed in the foreground. Running `ps` with no other arguments allows you to see all processes in the current shell. If you don't have any processes currently running in your shell background, your output should look similar to this:

```PID TTY          TIME CMD
  83144 pts/1    00:00:00 bash
  83343 pts/1    00:00:00 ps
```

To run processes in the background, the `&` (ampersand) character can be appended to a command to run it in the background. The current subprocess number, as well as system process id should be printed after running the command, for example `[1] 3779`. Try running `sleep 5 &` and `sleep 5` and notice the former allows you to still use the shell while it runs. Running `ps` soon after the backgrounded command should also show the `sleep` process in the shell process list.

**Note**: Even if processes are run backgrounded, output may be sent to the terminal, if you don't want to see the output consider hiding STDOUT and STDERR by using `&>/dev/null` to redirect the program's output.

To background a currently running process, we can suspend and background it. As an example. Let's run `sleep 15`, enter `Ctrl+Z` to suspend the program (there should be an output of the subprocess id, for example `[1]+`), then run `bg [SUBPROCESS ID]` (usually 1). Run `ps` again to see the process running in the background.

To foreground a process, run `fg [SUBPROCESS ID]`, feel free to try the above example again using `fg` instead of `bg`. This also works with processes currently backgrounded.

However, even if a process is backgrounded, closing the terminal will close any processes running from the shell, including applications started from the shell. To fix this, you can use `disown` to remove processes from the shell's job control list. To test this, let's run `nautilus &`, the default file manager for Ubuntu in the background (if this is unavailable, than any other application with a gui can be used to demonstrate. Close the shell window and notice that `nautilus` (or your chosen application) closes as well.
Next, open your shell window again, run `nautilus &`, then type `disown` to clear the shell's job control list. Close the shell window again and see that `nautilus` stays open.

**Note:** `disown` can also be used with a specific process id, for example `disown 5723` to remove a specific process from the shell job control list.


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
