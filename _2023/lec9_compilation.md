---
layout: lecture
title: "Getting Stuff you Download to Compile"
date: 2024-02-06
ready: false
phase: 2
---

## Interpreted vs Compiled Languages

There are two ways of running code, interpreted or compiled.

Each language usually only uses one type, some examples below:


### (mostly) interpreted languages:

- Python
- Ruby
- Javscript

### (mostly) compiled languages

- Java
- C
- Rust


## Running Interpreted Code

A program (interpreter) reads the human-written code in real time, analysing the text written and converting that into instructions that the interpreter executes as it reads them. Other code files can be dynamically referenced, utilising code from other project files or installed libraries.

This, however, means that errors in code won't be picked up until they're reached by the interpreter.


## Running Compiled Code

A compiled language needs to be compiled before it's run. This is done by a program (compiler), reading the human-written code and converting the text into object code, instructions that can be read and executed by the machine directly.

Object code is specific to the target operating system and hardware architecture, as the instructions are executed by the target kernel. The specificity of object code is why compilation needs to be done by the user after downloading the source code.

It is much faster to run the compiled code vs interpreted code as the text doesn't need to be lexically analysed as it runs.

Compilation can also be done for other machines with different specifications (cross-compiling), but some build systems may not allow this.



## How is code compiled?


Converting a set of source code files into one executable is done through building.

Building consists of compilation and linking:

- Compilation: conversion of source code files into a set of object code files

- Linking: conversion of a set of object code files into one executable


## Is it better to download executables or source code?


Downloading source code instead of compiled executables is much safer as the source code can be read through to ensure no malware is packaged with the files. This also allows source code to be read and edited to fit requirements before building, also can edited for debugging.


## Build Systems

Instead of having to deal with manually compiling, linking, and managing dependencies, build systems are available. These automatically detect hardware architecture and OS (if differences aren't specified), and build the source code files for the user automatically.

These often use specific data files that specify dependencies, versions, and how to compile and link the source code files.

Examples of common build systems:

- `GNU make`
- `msbuild`
- `bazel`
- `meson`
- `maven`
- `gradle`

When choosing a build system to use, try to prioritise a build system that is popular, well-suited to your most-used project programming languages, and versatile.


## How to use build systems

To find what build system a project uses, one good way is finding a file in the project root directory that often looks human readable and out of place with the others.

Build data file examples:

- `Makefile` (GNU make)
- `build.ninja` (ninja)
- `BUILD.bazel` (bazel)
- `PROJECT.csproj` (msbuild)
- `pom.xml` (maven)

Googling this will usually tell you how to build it, if the repository doesn't already specify the build instructions. Before building, the build system might need to be installed first, and the usage of the build system CLI can be found through either googling, or CLI explanation tools like `tldr` or `man`.

IDEs like VSCode will also often automatically detect build systems and files, and allow extensions/plugins to be installed to build the project in the IDE.

Building and running can also be done in containers like docker, and this is useful because building projects often requires downloading required dependencies. These can take up a lot of space on the host OS, so keeping them in a docker container stops more storage being used up when not required.

### Examples with common build systems


| Build System | Build           | Clean Existing Object Files | Build Debug        | Run Tests      |
| ------------ | --------------- | --------------------------- | ----------------   | -------------- |
| GNU make     | `make`          | `make clean`                | `make -d`          | N/A            |
| Maven        | `mvn install`   | `mvn clean`                 | `mvn -X install`   | `mvn test`     |
| Gradle       | `gradlew build` | `gradlew clean`             | `gradlew build -d` | `gradlew test` |

## Types of Executables

Executables can be built as standalone executables, or with dependencies. Standalone executables don't require dependencies to be installed on the host OS, as they include the dependency code inside them. Executable files with dependencies refer to installed libraries to access code, but these libraries need to be installed on the host OS to run.

Standalone executables are a lot more portable, but are larger than their dependency-reliant counterparts. They can be quicker and smaller to install if the dependencies aren't often used, but if many executables require these dependencies, then it's often more storage-efficient to use dependencies.

The type of executable can often be specified in the build options.


## Common Errors


### Dependency Errors

<div drawio-diagram="550"><img src="/static/media/2023/compilation/compiler-dependency-error.png"></div>

Dependency errors occur when the code being compiled cannot access a method, object, function, or class required to run. These are often resolved with some build systems installing dependencies for you, but if this isn't possible, manually installing the code dependencies with the language package manager of your choice is required.

Some examples of package managers for programming languages:

- `gradle` (java)
- `go modules` (go)
- `cargo` (rust)

However, the dependency might have to be installed via software package manager like `apt`, `yum`, `pacman`, or whatever's most used on your system. Your best bet for finding the dependency to install if the above two options don't work is, again google.


### Version Errors

<div drawio-diagram="550"><img src="/static/media/2023/compilation/compiler-version-error.png"></div>

Version errors occur when there is a mismatch between versions for the compiler, dependencies, or code itself. This can be resolved by specifying the versions of the affected code/software in the build options, and sometimes installing earlier versions of dependencies if later versions break the code.


### Other Errors

<div drawio-diagram="550"><img src="/static/media/2023/compilation/compiler-other-error.png"></div>

Most compilers will provide an error code along with the error, and googling this will most likely lead you to documentation, or other recommended solutions online.
