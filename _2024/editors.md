---
layout: lecture
title: "#4: Text Editors"
date: 2024-10-28
ready: true
---

# Writing and Manipulating Text

As programmers, we spend most of our time editing code, so it's worth
investing time mastering an editor that fits your needs. Here's how you
learn a new editor:

-   Start with a tutorial (i.e. this lecture, plus resources that we
    point out)
-   Stick with using the editor for all your text editing needs (even if
    it slows you down initially)
-   Look things up as you go: if it seems like there should be a better
    way to do something, there probably is

If you follow the above method, fully committing to using the new
program for all text editing purposes, the timeline for learning a
sophisticated text editor looks like this. In an hour or two, you'll
learn basic editor functions such as opening and editing files,
save/quit, and navigating buffers. Once you're 20 hours in, you should
be as fast as you were with your old editor. After that, the benefits
start: you will have enough knowledge and muscle memory that using the
new editor saves you time. Modern text editors are fancy and powerful
tools, so the learning never stops: you'll get even faster as you learn
more.

# Which editor to learn?

Programmers have [strong
opinions](https://en.wikipedia.org/wiki/Editor_war) about their text
editors.

Also, not all text editors are equal:

![A selection of \"editor learning curves\", that presumably graph
complexity over time. Emacs\' line spirals
nonsensically.](../files/editor_curves.png)

Today I\'ll be talking about Emacs - characterised in this diagram as
being a little esoteric, perhaps not entirely unfairly - a powerful
graphical and terminal-based editor. It is perhaps one of very few
exceptionally old editors - first released in 1984 - that still sees
modern development. Despite its age, it has a very capable editing
paradigm and support for modern affordances such as the latest code
analysis tools.

Despite this, you may find Emacs not to your taste. Many people don\'t!
Do not fret; what is most important that you find an editor that works
best for you. Emacs works best for me, and I\'m advocating that you try
it here because I feel that even if you do not like it, it might help
you to figure out what works for you.

## Why Emacs? Why not something shiny and modern?

![A comic showing people arguing over text editors. One describes
flipping bits manually through an absurd process, before another remarks
that Emacs has that functionality built
in.](../files/real_programmers.png)

You\'re probably wondering why I\'m talking about Emacs here; certainly
it is less popular than the venerable (and also excellent!) Vim, whose
advocates will extol the virtues of efficient modal editing and a
minimal interface. Emacs has neither of these things. You might have
heard of or used Visual Studio Code, arguably one of the most popular
text editors currently in use, with a real, shiny graphical interface
and the financial backing of Microsoft; certainly Emacs has neither of
these things either.

The fundamental strength of Emacs comes in two parts:

-   Buffer oriented interaction. This may seem unusual to anyone with
    experience in a graphical tabbed editor, but Emacs\' focus on
    buffers as the fundamental unit of organisation in the interface
    allows you to manage exceptional amounts of information with ease.
-   Flexibility and extensibility. Above all, this is Emacs \"killer
    feature\", if you look for such a thing. Emacs is designed in all
    aspects to be moulded to fit - you, the user; the task you are
    performing; the constraints you are performing it under. With enough
    experience you can bend Emacs into the needed or desired shape.

There is a reason that there are people that started using Emacs in the
order of decades ago and still use it today. Its long history means that
has become an exceptionally powerful tool to read, write, and manipulate
text - any text - especially code. It has many features that I will not
touch on here, but be assured that almost any task that demands you
manipulate textual data can be done efficiently in Emacs. Of course,
this presentation was prepared *entirely* within Emacs itself.

# A note on Emacs\' Odd-ness

Emacs is several decades old, and not making drastic changes to core
behaviour is highly prioritised by its developers. You will find many of
its keyboard shortcuts to differ from what you expect; they were decided
upon before the ones you know existed. Some of its defaults may seem
strange - there is a wealth of functionality that remains disabled by
default. I encourage you to try to familiarise yourself with these
oddities before you set about changing them; with time you may become
comfortable with them.

Emacs is exceptionally big: certainly I am not aware of all of its
functionality. Please try not to feel overwhelmed as you prod at it.
Even as someone who uses it very, very extensively, it is not unusual
for me to discover new functionality.

# Graphical or Textual

Hopefully you have now launched a copy of Emacs. Likely you are looking
at a graphical interface, with a menubar and toolbar full of buttons.
You might be looking at a purely textual application running within your
terminal, depending on the copy of Emacs you installed and how you ran
it.

The graphical interface has some niceties - the menubar and toolbar can
help you to discover functionality, and it supports images, mixed font
faces and sizes, etc. - things that a terminal is not capable of. Worry
not if you are looking at Emacs in a terminal, however - it is just as
powerful. In fact, running Emacs in the terminal is one of a few ways
you might to choose to use Emacs to edit files remotely.

# \"Basics\"

## Keyboard Shortcuts and Notation

Emacs relies heavily on the use of modifier keys to input commands.
Mostly, this is the CTRL key and META key (which you may better know as
\"ALT\"). Here we also use the Emacs format for representing key
sequences: successive strokes are space-separated, and if a modifier key
is used in a keystroke then it is prefixed with an abbreviation of that
modifier and a dash. For example:

-   `C-x b` means that you should hold the CTRL key while
    pressing `x`, and then release it before pressing
    `b`.
-   `C-x C-b` means that you should hold the CTRL key while
    pressing `x` and then press `b` before
    releasing it.
-   `M-b` means that you should hold the META key (remember,
    you may know this as the ALT key) while pressing `b`.

Some keys are represented by short sequences of letters:
`SPC` is the space bar, `RET` is the return key,
and `ESC` is the escape key.

Most shortcuts you will use will be preceded by `C-x` or
`C-c`, or consist of `M-<key>`. This is one of the
general rules that is followed by convention. Many shortcuts in Emacs in
mnemonic; this might help you to remember them.

## Don\'t Panic!

Sometimes you might get Emacs into a state that you don\'t understand,
especially when you are starting out. To cancel the current operation,
use `C-g`. You may need to press it several times to exit
nested operations. You can also use `ESC ESC ESC` to cancel
operations. This is a slightly \"stronger\" option than
`C-g`, so pressing your escape key a few times is an easy way
to exit some state and return to the main editing loop. Don\'t panic!

## Opening and Saving Files

Let\'s start by creating a new file. Press `C-x C-f`. You
should now see a prompt at the bottom of Emacs. Here you can open an
existing file, but we will create a new file. Enter \"test.txt\" at the
prompt. You should now see your blank file in front of you. Enter some
text and press `C-x C-s` to save it. You will soon learn more
about how you manage the files you have opened in Emacs.

## The Tutorial

Emacs has a nice (if quaintly antiquated) tutorial built in. Consider
using it; you can access it by pressing `C-h t`. Open it now;
we will navigate around it in the following sections.

## Movement

Emacs has an extensive set of movement commands, but many of the ones
you are familiar with will already work. Try using the arrow keys; Page
Up and Down; Home and End; and holding control with the arrow keys.
These should all behave how you expect them to. There are movement
commands that perform all of these actions without you having to reach
away to that side of your keyboard; of course there are also many more
movement commands. I touch on these here because they will make you able
to navigate documents quickly: do not worry about these for now. You
should be able to move through documents without too much difficulty
using the bindings you already know. Additionally, in graphical Emacs
(and terminal Emacs with some configuration), you can use the mouse as
you expect.

-   `C-a` will move to the start of the line.
-   `C-e` will move to the end.
-   `C-p` will move to the previous line.
-   `C-n` will move to the next.
-   `C-b` will move backwards one character.
-   `C-f` will move forwards one character.
-   `M-b` will move backwards one word.
-   `M-f` will move forwards one word.
-   `C-v` will move down one page.
-   `M-v` will move up one page.
-   `M-<` will move to the top of the document.
-   `M->` will move to the bottom.
-   `C-l` will recenter the window, adjusting what part of
    the document you can see. Try pressing it multiple times in
    sequence.

Many of these will also work in your shell by default, along with any
program that uses readline, such as many programming language REPLs. Try
it out!

## Buffers, Windows, and Frames

If you\'ve been following along, you should be looking at a single Emacs
\"frame\". This is the Emacs term for what you might call a \"window\".
Multiple frames can be associated with the same running copy of Emacs;
documents open in one are also accessible from another.

Inside this frame, perhaps confusingly, is a \"window\". You might be
more familiar with the idea of \"splits\" or \"panes\". Try opening a
second window with `C-x 3`. Now, within your single frame you
can see two windows, laid side-by-side.

Notice that when you did this you can see the same document in each
window. These documents are, in Emacs-speak, \"buffers\". Try scrolling
one of these windows and notice that the other does not scroll with it.
Here is a central insight into this user interface: frames contain
windows, and windows are view into a buffer. Windows can be views into
different buffers, or different views into the same buffer. Perhaps you
want to write some code and keep a view of a different part of that file
for reference, or perhaps there is a function taller than your screen
that you can see in its entirety when you have two windows.

When you use Emacs as a text editor, many of your buffers are
\"visiting\" files. This means that saving the buffer saves your changes
into that file. You will soon encounter buffers that are not visiting
files; for example, when you interrogate Emacs about what a key does
with `C-h k <key>`, it will tell you what that key does by
displaying a buffer that describes it. That buffer is not visiting any
file.

### Managing Buffers

To change the buffer that the current window is displaying, you can use
`C-x b` - think, `b` for buffers. You can see by
default it will toggle to the last used buffer if you were to just hit
`RET`. Hit `TAB` to see completions; entering one
of these and hitting `RET` will switch to it. Finally, if you
would like an overview of all of your buffers, use `C-x C-b`.
This will show a list you can navigate and you can use `RET`
to switch to the buffer under the point.

You will also want to \"kill\" buffers, which you may better know as
\"closing\". This can be done with `C-x k`. The default
behaviour is to kill the current buffer, which can be done by hitting
`RET`. As above, you can complete and kill other buffers too.

### Managing Windows

Emacs has a set of less intuitive bindings for managing windows:

-   `C-x 1` will close all windows except the current one.
-   `C-x 2` will split the current window, opening a new one
    below it.
-   `C-x 3` will split the current window, opening a new one
    to the right of it.
-   `C-x 0` will close the current window.
-   `C-x o` will cycle the currently active window. In
    Emacs-speak, it selects the \"other\" window.

Remember: windows are only views into buffers. If you want to kill a
buffer, you do that with `C-x k`.

## Point, Mark, and Region

You are likely familiar with the \"cursor\" - this is where text will be
inserted. In Emacs-speak, this is the \"point\". There exists another
marker in every buffer, the \"mark\". The mark is not visible but many
commands will place it. The part of the buffer that is between the point
and mark is known as the \"region\". When you manually place the mark
with `C-SPC`, Emacs will interactively highlight the region,
which is now \"active\"; this is similar to highlighting text in other
editors. Many commands will operate on the active region as you might
expect. We will touch on commands later, but the command
`downcase-region` - for example - will make all uppercase
characters in the current region lowercase.

Try this out: set the mark with `C-SPC`, then \"deactivate\"
it with `C-g`. The mark is still set, but no longer active.
Move elsewhere in the document and then press `C-u C-SPC`.
The point will jump immediately to the mark you placed. Many of the
movement commands above also set the mark. Try hitting `M-<`
and then jump back to where you were with `C-u C-SPC`.

## The Minibuffer and Echo Area

At the bottom of the current frame, you can see the echo area. Emacs
will surface messages to you here to notify you of events. Some commands
will turn this area into the minibuffer. Here you can enter text into
the minibuffer as the command requires. With it open, use
`TAB` to trigger completion. When you opened a file earlier,
you used the minibuffer. Let\'s take a look at one of the most important
commands that makes use of the minibuffer.

## execute-extended-command

Try pressing `M-x`. Here you are being prompted for a command
to execute. Let\'s try running `beginning-of-buffer`. Wait a
second - isn\'t that what `M-<` does? Yes!

Every shortcut you have seen is actually bound to a command with a name.
Some commands are not given a shortcut; the only way to execute them is
with `M-x`. Why is this section called
`execute-extended-command`? Because `M-x` is
itself also bound to a command; this command is
`execute-extended-command`. If you forget a shortcut, you can
try running the command it is associated with. Often if you are
instructed to run a command it may be written
`M-x beginning-of-buffer`. This instructs you to press
`M-x` and then enter the command
`beginning-of-buffer`.

## Modes

Emacs has a concept of \"modes\". Every buffer has a \"major\" mode. The
major mode governs the broad behaviours of Emacs for that buffer. For
example, if you were writing some C, you would want to use the
`c-mode` major mode. This tells Emacs how to highlight,
indent, comment, etc. the code you are writing. Were you writing a shell
script, `sh-mode` would be more appropriate. A buffer can
only be in a single major mode.

There are also \"minor\" modes - some buffer-local, and some that modify
the global behaviour of Emacs. Many minor modes can be active at once.
For example, `flyspell-mode` checks your spelling on the fly;
there is also `flyspell-prog-mode` for only checking strings
and comments as defined by the current major mode. Notice how these
things compose. One global minor mode - if you have Emacs 28.1 or
newer - is `fido-vertical-mode`. Try executing the
`fido-vertical-mode` command with `M-x` and then
hit `M-x` again. The entire minibuffer selection interface is
now fuzzy-matching and live-updating, and not just for `M-x`!
Try `C-x C-f`!

## Kill and Yank

In a typical editor, you may be familiar with \"copying\" and
\"pasting\". Of course Emacs has these ideas, and of course they work
differently to how you may expect. In Emacs there exists the \"kill
ring\" - you may better know it as the \"clipboard\". When you \"kill\"
text, it is copied to the kill ring. You may then \"yank\" text from the
kill ring into the current buffer, which is analogous to pasting.

-   `C-w` will kill the current region. This is similar to
    the \"cut\" operation you may be familiar with.
-   `M-w` will save the current region to the kill ring
    without actually killing it. This is similar to the \"copy\"
    operation.
-   `C-y` will yank the most recently killed text from the
    kill ring and insert it into the current buffer. This is similar to
    the \"paste\" operation. Wait a second-- most recent?
-   `M-y` will allow you to yank previously killed text from
    the kill ring; the kill ring in Emacs is not just a clipboard but
    also stores history.

In graphical Emacs, the kill ring and system clipboard are kept
synchronised, so interacting with other applications works as you
expect.

## The Modeline

At the bottom of each window is the Modeline. It describes the current
state of the buffer that window is showing: it\'s name, if it modified
from the content on-disk, the encoding, its major mode, and active minor
modes. It can be extensively customised.

## isearch

Emacs has a powerful in-buffer incremental search. Invoke it with
`C-s`. you may enter your search term, and move to matches by
pressing `C-s` again. `C-r` may be used in the
place of `C-s` to search in reverse. Note how it is
case-insensitive unless you enter any uppercase character. Exit with
`C-g`.

While you are in isearch, there are various ways to modify the the
behaviour of the search. If you would like to use regex, use
`M-s r`. Emacs can also helpfully display an overview of all
occurrences of your search term. Hit `M-s o` and an occur
buffer will appear; use `RET` to jump to the location of the
match under point in the source buffer.

It is also possible to isearch across multiple buffers, where this
behaviour also works.

## Completion

Emacs has a completion system built in, called
`completion-at-point`. You can invoke it with
`M-TAB` or `C-M-i`, although `M-TAB`
may not work with the textual interface depending on your terminal
emulator. It can pull from many sources; one of these is the LSP
mentioned below.

## LSP

I touch here briefly on LSP (Language Server Protocol) support in Emacs.
LSP powers modern code analysis in editors such as VSCode, and Emacs 29
ships with support for communicating with the very same LSP servers,
achieving the same level of code analysis within Emacs. It is accessible
with `M-x eglot`; upon running this command Emacs will use
the major mode of the current buffer to choose and run an appropriate
language server. You will need to install these language servers
separately, preferably with your system package manager, before Emacs
can run them.

## Remote Editing

One thing that Emacs, like Vim, excels at is remote editing.

![A figure from a book, captioned \"Remote login is a lot like astral
projection\". It depicts a person at a computer magically projected to
be sitting in front of another.](../files/astral_ssh.jpg)

### With the textual interface

As mentioned, Emacs can run inside of the terminal. Launching while you
are connected to a remote over ssh, for example, will automatically
select the terminal interface. Here you can use it exactly as if it were
on your own computer. Unlike Vim, however, Emacs is not ubiquitously
installed. You may find it available, or you may have to install it.
Additionally, it will run with its default configuration; you would have
to copy your own configuration over if you need your changes to be able
to use Emacs well. This is a reliable way to use Emacs remotely, and you
may choose to combine it with a terminal multiplexer too.

### With \"Transparent Remote Access\"

Emacs itself also has the ability to transparently work on remote
systems as if they were local. You can list and edit files as if they
were on your own computer, and you can use the graphical interface
should you prefer it. This is an excellent option, but only if you on a
fairly fast connection to the remote. On a slow connection, Emacs will
unhelpfully multiply these performance issues and make them quite
frustrating.

Emacs uses a system of \"transports\" to implement this behaviour. The
most useful will likely be the \"ssh\" transport, which - as you would
expect - will connect to a remote over ssh. Emacs has no need to install
anything on the remote in this operation - it needs only a few
essentials which will almost certainly be installed. I am yet to connect
to a system that didn\'t have them.

Additionally, there are many other transports. Would you like to edit a
system file with `sudo`? Instead consider the \"sudo\"
transport. Update your website over FTP? Use the \"ftp\" transport.
Manipulate data within a Docker container with all the luxury of your
regular Emacs configuration? Use the \"docker\" transport. Best of all,
you can combine these transports. Want to edit a system file on a remote
host, but root login is disabled? Combine the \"ssh\" and \"sudo\"
transports to pull it directly into your graphical Emacs running on your
own computer.

You can interact with this system with a slightly odd syntax in the file
picker. Let\'s ssh into the remote \"pwnie\" by hitting
`C-x C-f` and entering:

    /ssh:pwnie:

Now we can see a directory listing. Let\'s open our
`.bashrc`. We could also combine these transports.
`/etc/sudoers` is not readable by us normally:

    /ssh:pwnie:/etc/sudoers

But if we combine it with the \"sudo\" transport:

    /ssh:pwnie|sudo::/etc/sudoers

It\'s right here for us!

# \"The self-documenting editor\"

Emacs is surrounded by a pervasive documentation culture. The core
editor and the massive number of packages available for it are often
thoroughly documented. Most importantly of all, Emacs is designed to be
\"self-documenting\" - you can use Emacs to learn more about Emacs. Once
you are comfortable enough with Emacs to use it in this way, you will
find that you can often learn the information you need without leaving
your editor.

## `C-h`

Behind the `C-h` prefix there are a wealth of commands to
help you interrogate Emacs.

-   `C-h t` will open the tutorial.
-   `C-h f` will describe any function (including commands,
    which are simply interactive functions).
-   `C-h v` will do the same for variables, which are often
    control configuration.
-   `C-h o` will do the same for functions and variables
    simultaneously. One name can have both a variable part and function
    part; this is a consequence of Emacs\' lisp-y nature that we touch
    on later.
-   `C-h k <key>` will describe the key sequence
    `<key>`. Want to know more about the command executed
    when you hit a keyboard shortcut? Use this!
-   `C-h b` will describe the currently active key bindings.
    This isn\'t just a fixed list - it will show you which modes are
    responsible for which bindings.
-   `C-h C-h` will show you help-about-help, just in-case you
    get very lost!

## Info

Sometimes you need more than descriptions of individual bits of
functionality - you want a structured guide. Emacs is documented with
the GNU `info` system, and does of course come with an info
viewer that you can use to read it. Open it with `C-h i`.
Here you have extensive documentation for Emacs and its many components
alongside other info pages you have installed. Later I will talk about
Emacs Lisp, for which there is a full, guided introduction in this info
system.

## Emacs is Introspective

Finally, I\'d like to emphasise the most important quality of
interrogating Emacs to explain Emacs: it is introspective. If you
install a new package, you can interrogate Emacs about it as if it were
part of Emacs itself. These packages can also install their own info
pages. Your copy of Emacs doesn\'t just document Emacs; it documents
*your copy* of Emacs.

# \"The extensible editor\"

## Packages

Emacs has a very, very long legacy of packages. It has shipped with a
built-in package manager for some time now, and you can use this to
install new packages. Emacs can be used as much more than just an editor
if you find it comfortable - for me, it is also a git client, mail
client, RSS reader, chat client, terminal,
organiser/agenda/calendar/todo manager, notification centre, calculator,
PDF viewer, music player, web browser, hex editor, debugger interface,
and more. If there is something that you would like Emacs to do,
somebody has probably written a package to do it. If not, then you can
extend Emacs yourself!

Perhaps you like the modal editing of Vim? Try evil-mode, or another
modal-editing package! I have not touched here on some of the excellent
packages that keep people using Emacs even if they prefer editor: Magit,
an excellent interface to git; org-mode, a very powerful combined
document system, agenda, TODO manager, literate programming environment,
and more; and dired, a file manager that also allows you to manipulate
files like text; and more! Two of these three are already built-in to
Emacs!

## Emacs Lisp

Emacs Lisp - or \"elisp\" in short - is the language in which most of
Emacs is implemented in. It is the language that every package is
written in, and it is the language that, should you want to extend Emacs
yourself, you would write in. Importantly, Emacs does not really
distinguish between elisp that is used to implement itself and your
elisp. Unlike, say, VSCode, you are not limited to some \"extension
interface\" - your elisp is just as important as core editor
functionality. If you want some behaviour, it may only be a few lines of
elisp away. Unless you have written some Lisp before, you will probably
find it quite strange; this isn\'t unusual!

I will not discuss elisp here in detail - that would require another
session entirely! If you choose to use Emacs more, it is not required
that you learn how to write elisp. However, it is an exceptionally
powerful way to manipulate Emacs, and for me, what gives it its staying
power. You should be able to find a tutorial within the info viewer.

# Wrapping up

Sadly, there is a quite a bit of Emacs functionality that has not been
touched on here. However, I hope that with a glimpse into this deep pit
that you are equipped to try and learn more about Emacs by yourself. Of
course, if you have questions I encourage you to reach out! Emacs has
been an excellent tool for me and I\'m happy to help if you think it
might work for you too.
