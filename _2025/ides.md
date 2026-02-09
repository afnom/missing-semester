---
layout: lecture
title: "#10: Introduction to IDEs"
date: 2026-02-09
ready: true
hide: false
---

## Context: Using IDEs

Throughout the missing semester, we have interfaced with various command line utilities:
The shell, editors, version control systems, container solutions, compilers, debuggers, and more.

Being proficient with all of these tools from the command line itself greatly helps our understanding of how individual tools work.
This knowledge can be highly influential for our day-to-day activities as computer scientists, especially when we are working on remote machines without any attached UI.

However, sometimes, and based on personal preferences, it may be convenient to interface with these tools through a graphical UI, rather directly from the command line.
This is where *IDEs* (or, in long: Integrated Development Environments) come in: These are software applications aiming to ease the software development process.
Oftentimes, IDEs combine a graphical text editor with various interfaces to the different command line utilities we discussed so far.
For instance, they may provide text editing capabilities with quality-of-life features such as syntax highlighting together version control via git and interfaces to commodity compilers & build toolchains.

These notes introduce the topic of IDEs, discuss common IDEs and features, and list some common extensions and shortcuts. However, due to the constraints of demonstrating a GUI-based application, these are not complete and instead serve more as a supplement for the proper talk, rather than a comprehensive substitute. For example, the demos are almost entirely talk-only. If you are reading these notes and did not or will not attend the talk, many guides exist online to introduce any VS Code feature you could imagine and can be used in place of my demos.

## Choice of IDE

Should you choose to use an IDE, you will subsequently need to pick which one you want to use. Generally, this will depend on what features you want and what language you want to work with. It's also worth considering that some IDEs are entirely free and open source, some are freeware, and some are paid products - your needs and budget will determine what to go with.

For example, if you're planning to work with Python, your install likely came with Python's very own IDLE (Integrated Development and Learning Environment) - a barebones IDE that is easy to get going with. While this may suffice for beginner users, it lacks a lot of the fancier and more powerful features that many more advanced users want, such as autocompletion and bracket-matching. Therefore, you might opt to use JetBrains' PyCharm, a common pick for Python developers, either at the free tier or the paid "Pro" version. Alternatively, you might choose to go with a more generic IDE that supports Python, such as Visual Studio Code (VS Code).

We will be looking at Visual Studio Code during this session, since it is both powerful and versatile (and also my personal choice for most tasks). While VS Code uses an open-source core, if you want open-sourced builds without Microsoft's hidden additions, try VSCodium.

### Common laternative IDEs
Below is a short-list of alternative IDEs for various languages. This is in no way an exhaustive list, and is offered to serve as a basic overview should you wish to try an alternative IDE.

- JetBrains' suite (free + pro options)
  - IntelliJ IDEA (Java)
  - Rider (.NET)
  - WebStorm (JS + TS)
- FOSS Options
  - Eclipse (Java)
  - NetBeans (Java)
  - Atom/Pulsar (General)
  - Geany (General)
- Others
  - Visual Studio (.NET)
  - Cursor (AI-driven)

## Visual Studio Code

Visual Studio Code is an IDE developed by Microsoft which supports a large range of languages out-of-the-box, and even more through the popular extensions marketplace. In fact, these notes were written using VS Code! It is lightweight (compared to many other IDE offerings), yet has a wide range of the features you'd expect from a modern, popular IDE - code completion, bracket-matching, syntax highlighting, integrated building and execution of code, etc. It even comes with integration for "Chat", an AI agent (opinions on that left as an exercise for the reader...).

### Demo - basic overview

- Basic panel layout and overview
- File editing
- Search (and replace)
- Git integration
- Extensions marketplace
- Settings
- Word wrap
- Make Python venv within VS Code

## Useful Windows/Hotkeys
- Explorer (`ctrl+shift+e`): The "standard" view: looking at the different directories files in your project.
- Terminal (`` ctrl+` ``): VScode has a built-in terminal, allowing us direct access to all the tools we looked before!
- Command-Palette (`ctrl+shift+p`): A dialog with search bar to execute various commands
- Search (`ctrl+shift+f`): Search over all the files in the project
- Source control (`ctrl+shift+g`): Allows us interface with the source control system used by the project (e.g., git).
- Extensions (`ctrl+shift+x`): vscode versatility stems mostly from its extensive plugin/extensions system. Using this, we can extend the feature set of the IDE.

## Useful Extensions
- rust-analyzer
- Pylance/Python Debugger
- Markdown Preview
- Jupyter
- Hex Editor
- Docker
- Gradle
- Dev Containers
- Remote-SSH
- Latex Workshop

### Demo - dev containers
- Enables development entirely within container
  - All the advantages of Docker as explained previously!
- Can even debug remotely

### Security Notice

VS Code's extensions are fantastic and powerful but care should be taken when installing and using them. While some authors are marked as verified owners of a domain (e.g. Microsoft's official extensions, VueJS, etc.), the extension marketplace is just that - a community marketplace where anyone can upload their own code for others to use. There is no requirement for authors to make their code open source, so effectively you are blindly running others' code - be careful! While ratings, age, and download counts can indicate whether an extension is safe, do not rely on these - manage your own risk level. Attacks via these extensions [can and do occur](https://www.bleepingcomputer.com/news/security/malicious-vscode-extensions-with-millions-of-installs-discovered/)! (Especially for [AI-themed extensions](https://www.bleepingcomputer.com/news/security/malicious-ai-extensions-on-vscode-marketplace-steal-developer-data/))

## Conclusion

IDEs are a personal choice - you make like a more full-on, heavier IDE or you may like a more lightweight IDE that mostly stays out of your way. You might religiously use one IDE for every task, or you might switch depending on your language and project at the current time. Most IDEs are extensible, and you can often customise them to whatever extent you wish. Play around with some different options, find what works for you, and find your productivity spike! (Hopefully. Missing Semester does not make any guarantees of an improvement in productivity 😄)