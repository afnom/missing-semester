---
layout: lecture
title: "#3: Editors (Emacs)"
date: 2023-10-17
ready: true
phase: 1
video:
  aspect: 56.25
  id: a6Q8Na575qc
---


<div class="note">
The video above is part of the original MIT Missing Semester recordings.  <br><br>
While the UoB version of this session will cover the same base material, please expect some differences during the live session.
<br>
</div>

<https://xkcd.com/378/>

# Writing with a computer

Writing English words and writing code are very different activities. When programming, you spend more time switching files, reading, navigating, and editing code compared to writing a long stream. It makes sense that there are different types of programs for writing English words versus code (e.g. LibreOffice Writer versus Emacs).

As programmers, we spend most of our time editing code, so it's worth investing time mastering an editor that fits your needs. Here's how you learn a new editor:

Start with a tutorial (i.e. this lecture, plus resources that we point out)
Stick with using the editor for all your text editing needs (even if it slows you down initially)
Look things up as you go: if it seems like there should be a better way to do something, there probably is
If you follow the above method, fully committing to using the new program for all text editing purposes, the timeline for learning a sophisticated text editor looks like this. In an hour or two, you'll learn basic editor functions such as opening and editing files, save/quit, and navigating buffers. Once you're 20 hours in, you should be as fast as you were with your old editor. After that, the benefits start: you will have enough knowledge and muscle memory that using the new editor saves you time. Modern text editors are fancy and powerful tools, so the learning never stops: you'll get even faster as you learn more.
Which editor to learn?

Programmers have strong opinions about their text editors.

Also, not all text editors are equal:

Which editors are popular today? See this Stack Overflow survey (there may be some bias because Stack Overflow users may not be representative of programmers as a whole). Emacs is one of the most popular and versatile editors available.
Why text-based editors? Why Emacs?
Emacs has a rich history; it was first developed in 1976 by Richard Stallman and Guy Steele. It's known for its extensibility and customization, making it a powerful tool for programmers and writers alike.

Emacs has some really neat ideas behind it, and for this reason, lots of tools support an Emacs emulation mode (for example, over 1 million people have installed Emacs emulation for VS code). Emacs is probably worth learning even if you finally end up switching to some other text editor.

It's not possible to teach all of Emacs' functionality in 50 minutes, so we're going to focus on explaining the philosophy of Emacs, teaching you the basics, showing you some of the more advanced functionality, and giving you the resources to master the tool.

# Philosophy of Emacs

When programming, you spend most of your time reading/editing, not writing. For this reason, Emacs is a modal editor: it has different modes for inserting text vs manipulating text. Emacs is highly programmable (with Lisp and other languages), and its interface itself is like an extension of Lisp: key combinations (with mnemonic names) are commands, and these commands are composable. Emacs avoids the use of the mouse extensively, focusing on keyboard shortcuts for efficiency.

The end result is an editor that can match the speed at which you think.

# Modal editing

Emacs' design is based on the idea that a lot of programmer time is spent reading, navigating, and making small edits, as opposed to writing long streams of text. For this reason, Emacs has multiple operating modes.

# Main modes

Insert: for inserting text
Normal: for moving around a file and making edits
Visual: for selecting blocks of text (line, column, or rectangular)
Emacs has some other less extensive modes as well that we'll talk about:

Replace: for replacing text
Minibuffer: for entering commands, prompts, and search terms
Shell: for running a shell command within Emacs
Keystrokes have different meanings in different operating modes. For example, the letter "a" in Insert mode will just insert a literal character 'a', but in Normal mode, it will move the cursor to the beginning of the current line, and in Visual mode, it will select the entire line.

# Basic Emacs usage

Inserting text

From Normal mode, press i to enter Insert mode. Now, Emacs behaves like any other text editor, until you press <Esc> to return to Normal mode. This, along with the basics explained above, are all you need to start editing files using Emacs (though not particularly efficiently, if you're spending all your time editing from Insert mode).

## Buffers, windows, and points

Emacs maintains a set of open files, called "buffers". An Emacs session has a number of windows (split panes). Each window displays a single buffer. Unlike other programs you are familiar with, like web browsers, there is not a 1-to-1 correspondence between buffers and windows; windows are merely views. A given buffer may be open in multiple windows, even within the same frame. This can be quite handy, for example, to view two different parts of a file at the same time.

By default, Emacs opens with a single frame, which contains a single window.

## Point and Mark

The point is the cursor in Emacs, indicating where the next insertion or deletion will occur. The mark is another position in the buffer that can be used for various operations, such as copying or deleting text. You can set the mark using various key combinations depending on the mode.

## Navigation

You should spend most of your time in Normal mode, using movement commands to navigate the buffer. Movements in Emacs are also called "nouns", because they refer to chunks of text.

### Basic movement

h, j, k, l (left, down, up, right)
w, b, e (next word, beginning of word, end of word)
0, ^, $ (beginning of line, first non-blank character, end of line)
Ctrl-u, Ctrl-d (scroll up, scroll down)
M-g (go to the beginning of the buffer)
M-G (go to the end of the buffer)
C-x <line number> (go to a specific line number)
There are many other movement commands available, including searching for text, finding specific characters or symbols, and moving by paragraphs or sentences.

## Editing

Everything that you used to do with the mouse, you now do with the keyboard using editing commands that compose with movement commands. Here's where Emacs' interface starts to look like a programming language. Emacs' editing commands are also called "verbs", because verbs act on nouns.

d (delete)
y (yank - copy)
p (paste)
M-x undo (undo the last change)
M-x redo (redo the last change)
C-k (kill - delete the line from the point to the end)
Many editing commands can be combined with counts and movement commands to perform more complex operations. For example, 3M-x delete-paragraph would delete the next three paragraphs.

## Customization

Emacs is highly customizable through its built-in Lisp interpreter. You can write Lisp code to extend Emacs functionality in various ways, including:

Defining new keybindings
Creating custom commands and functions
Modifying the user interface
Integrating with other tools and languages
There are also many pre-written packages and configurations available online that you can install to add new features to Emacs.

# Resources

Emacs Tutorial: This interactive tutorial is a great way to learn the basics of Emacs.
GNU Emacs Manual: The official Emacs manual is a comprehensive reference for all things Emacs.
Emacs Wiki: The Emacs Wiki is a community-maintained resource with a wealth of information about Emacs.
Planet Emacs: Planet Emacs is a blog aggregator that collects news and articles about Emacs.
By investing time in learning and customizing Emacs, you can turn it into a powerful and efficient tool that meets your specific needs and preferences. Remember, the learning curve can be steep at first, but with dedication and practice, you can unlock the full potential of this versatile editor.
