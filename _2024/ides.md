---
layout: lecture
title: "#11: Introduction to IDEs"
date: 2025-02-03
ready: true
---

### Context: Using IDEs

Throughout the missing semester, we have interfaced with various command line utilities:
The shell, editors, version control systems, container solutions, compilers, debuggers, and more.

Being proficient with all of these tools from the command line itself greatly helps our understanding of how individual tools work.
This knowledge can be highly influential for our day-to-day activities as computer scientists, especially when we are working on remote machines without any attached UI.

However, sometimes, and based on personal preferences, it may be convenient to interface with these tools through a graphical UI, rather directly from the command line.
This is where *IDEs* (or, in long: Integrated Development Environments) come in: These are software applications aiming to ease the software development process.
Oftentimes, IDEs combine a graphical text editor with various interfaces to the different command line utilities we discussed so far.
For instance, they may provide text editing capabilities with quality-of-life features such as syntax highlighting together version control via git and interfaces to commodity compilers & build toolchains.

In this session of the missing semester, we will look at one especially versatile IDE: Visual Studio Code.
Unfortunately, as this is a graphical tool, we will only recap some of the key points (and key combinations) in this online post, although we will showcase the different aspects in more detail during the life session.


### Important Windows/Hotkeys
- Explorer (`ctrl+shift+e`): The "standard" view: looking at the different directories files in your project.
- Terminal (`` ctrl+` ``): VScode has a built-in terminal, allowing us direct access to all the tools we looked before!
- Command-Palette (`ctrl+shift+p`): A dialog with search bar to execute various commands
- Search (`ctrl+shift+f`): Search over all the files in the project
- Source control (`ctrl+shift+g`): Allows us interface with the source control system used by the project (e.g., git).
- Extensions (`ctrl+shift+x`): vscode versatility stems mostly from its extensive plugin/extensions system. Using this, we can extend the feature set of the IDE.

## Extensions
During the session, we will look in a couple of extensions, tying back to the different aspect of the missing semester lectures so far:
- remote-ssh: Allows us to interact via SSH with remote machines
- dev containers: A easy-going interface to interact with docker containers (also works on top of SSH!)
- latex workshop: Tooling to work with latex documents.

