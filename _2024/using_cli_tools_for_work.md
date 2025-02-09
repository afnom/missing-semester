---
layout: lecture
title: "#14: Using CLI tools at your job"
date: 2025-02-24
ready: false
---

## Introduction

Welcome! This talk covers how many of the tools and ideas that you have been introduced to can be used in an industrial setting.

These skills have served me well since I first picked up Linux over a decade ago. I'm going to cover 10 overarching themes in this talk, with an emphasis on topics that have received a large amount of interest from course participants. These are:

- CLI Tools and Text Editors
- SSH for System Administration
- Version Control in Industry
- Continuous Integration and Delivery
- Documentation and Project Management
- LaTeX for Industry/Academia
- Web Application Frontends
- Database Design
- Cloud Provider Managed Services
- Cloud Automation Tools (Kubernetes + Terraform)

This is an incredibly broad (and deep) range of topics and some of you may not end up in roles where you can make use of all of them. However, I aim to show that the ways of thinking about problems that have been introduced in this course make it easier to approach new challenges and technologies. While the landscape of software and technology changes very quickly, investment in the core skills that we've covered in this lecture series is likely to remain relevant for many years to come:

- The first [Bourne Shell](https://en.wikipedia.org/wiki/Bourne_shell) appeared on V7 Unix in 1979
- [Emacs](https://www.gnu.org/software/emacs/) and [Vim]() both started development in the 1970s
- [TeX](https://en.wikipedia.org/wiki/TeX) was originally released in 1978
- Version Control Systems (VCS) have a long history, but [Git](https://git-scm.com/) has been in use since 2005 and is now near-ubiquitous
- It would still be wise to bet on mainframe systems and COBOL being in use for some time yet!


## Background

I have been working as a software engineer for over five years now, and prior to this I have been a heavy user of Linux and CLI tools in my academic and personal projects. I am in my final two years of a part time PhD in "Next Generation Railway Track Condition Monitoring" at the University of Birmingham [Centre for Railway Research and Education](https://www.birmingham.ac.uk/research/railway).

I will be sharing some of the experiences that have helped me in my time working on the projects that have been the focus of the last five years of my life. While many engineers move between projects frequently, I have had the luxury (or curse...) of working on the same projects over a long period of time, which has helped me to build a deep understanding of the systems and processes that I manage. I was also fortunate enough to pass the [AWS Solutions Architect Professional](https://aws.amazon.com/certification/certified-solutions-architect-professional/) exam building on skills I learned on the job.

While I will be relying on my experience, I am going off your feedback for what you are most interested in covering. Based on current polling results, there is most interest in CI/CD tooling, cloud automation and documentation and project management. This makes sense, as these are the areas that are least likely to be encountered until you are working in industry in a team. While there is little interest in frontend development, along with other tools that already been covered on this series, I will briefly touch on my experiences in those topics and how it fits into the overall story.


## CLI Tools and Text Editors

Your choice of text editor is likely to be one of the most impactful decisions you make in your career.

While we can all enjoy arguing about which text editor is superior, the most important thing is to allow teams freedom of choice - you should not depend on a specific editor in order to work on your project or do your job. Editors such as [Emacs](./emacs.org) and [Vim](./editors-notes.txt) allow an unlimited amount of customisation to improve your workflow whereas editors such as [VSCode](https://code.visualstudio.com/) and other IDEs tend to be more opinionated in how they are configured.

Regardless of your choice, focus on tools, not editors. Have standard, easily-installable commands to run tasks on the project, such as compiling, linting, testing, and generating documentation.

Many tools that you may wish to use allow *project-specific configuration*. This can be very useful if you work on a lot of projects. For exmple, you may [customise Git configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) to use different SSH or GPG keys, or a different name and email address, for specific projects. Many different ecosystems have a `standard' project configuration file e.g. [package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json/?v=true) for Node.JS, [pyproject.toml](https://peps.python.org/pep-0621/) for Python, [Cargo.toml](https://doc.rust-lang.org/cargo/reference/manifest.html) for Rust. These combine with ([theoretically](https://xkcd.com/927/)) standardised build tools. It is worth researching current best-practices for your chosen language or environment - e.g. [uv](https://docs.astral.sh/uv/) is gaining momentum in the Python landscape over tools such as [Poetry](https://python-poetry.org/).

Older languages e.g. C, C++ may not have a standardised project configuration file. Many of these tools use *build systems* like [good old-fashioned Make](https://www.gnu.org/software/make/) or more modern build systems like [CMake](https://cmake.org/) or [Ninja](https://ninja-build.org/). Regardless, it is important to document how to perform common tasks in a project to make it [easier for people to start working on your project](./open_source_dev_with_git.md).

I've recently had success with multi-platform teams using a cross-platform command runner called [Just](https://github.com/casey/just), which we use to run common commands. For example, for a software project you can have:

- a *check* command to validate that all dependencies are installed
- an *install* command to install the software locally
- a *test* command to run tests against the software
- a *build* command to compile or package the software

This has replaced Makefiles for a lot of use-cases, as it is relatively simple to make it work on MacOS and Windows as well as Linux. However, Make is still a good choice for running common tasks if you are sure your team will have it installed.


## SSH for System Administration

<div class="note">
Refer to <a href="./course-shell#ssh">Lecture 1</a> for an introduction to Secure Shell (SSH).
</div>

SSH is used for logging in to remote servers, whether they are on your home network, university network or anywhere on the internet (e.g. a Virtual Private Server (VPS) provisioned from a cloud provider).

It allows you to have a shell session on any machine and the flexibility of the Linux / UNIX design philosophy means that there is (usually) no difference between running commands on your local machine or on a server.

### Server Management

As you begin to manage servers you may struggle with keeping different operating system versions, installed packages, storage volumes, configuration or custom applications up to date. I recommend being as *disciplined as possible* in managing your personal servers and any server that you use for work or research.

One useful way to do this is to create [custom SSH aliases](https://wiki.debian.org/SshAliases) in your `$HOME/.ssh/config` file. This means that you only need to enter `ssh myserver` rather than `ssh username@long-domain-name-or-ip-address.org`. For example:

```conf
Host myserver
    Hostname 123.123.123.123
    Port 22
    User myname
```

When you have multiple servers to manage, this becomes extremely useful. However, it only works with the `ssh` command and related commands that use SSH under the hood, such as `sftp`, `scp` and `rsync`.

Another way of labelling servers requires editing `/etc/hosts`. This requires root privileges so you will need to use `sudo`. In general, you can avoid editing `/etc/hosts` as most of the time you will only be connecting to servers over SSH but there are some cases where you would want to, e.g. hosting websites on a machine on your home network.

Constantly connecting to servers to install updates is a laborious task. You can configure *unattended upgrades* (see [example for Debian](https://wiki.debian.org/UnattendedUpgrades)) which will keep the system up-to-date with security updates and patches. This is limited, however, as you will be required to reboot the server manually to apply kernel updates and distribution upgrade when required.


### Text Editing

Your favourite IDE may allow you to edit files over SSH (e.g. [VsCode](https://code.visualstudio.com/docs/remote/ssh), [Emacs](https://www.emacswiki.org/emacs/TrampMode)). However, it is often quicker to use terminal tools directly on the server to edit configuration and scripts.

[Vi](https://en.wikipedia.org/wiki/Vi_(text_editor)) is (pretty much) guaranteed to be on any UNIX server you work with as it is standardised in the [Single UNIX Specification](https://en.wikipedia.org/wiki/Single_UNIX_Specification). Its keybindings can take some getting used to, and many system administrators may prefer to use Nano (see [Lecture 2](./shell-intermediate.md)). However, there will likely come a time when you will need to use `vi` to edit a file and you have no alternatives, so it is well worth learning them even if (like me) you prefer [Emacs](./emacs.org).

The ability to SSH onto a random server and be able to quickly edit configuration files using a program that is *basically guaranteed to be present* is incredibly powerful!



### Securing your Servers

Any computer connected to a network is a vulnerable computer.


#### SSH

You will usually expose port 22 on a remote server to allow SSH connections. It is possible to change the port number exposed by SSH by editing `/etc/ssh/sshd_config`:

```conf
#Port 22    # change this line to
Port 54321  # a high-number port
```

This will significantly reduce the number of malicious attempts to log in, but is an example of [security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity) and there are more reliable ways to secure your servers. You can still choose to do this and, in combination with custom Port numbers in `$HOME/.ssh/config`, it is fairly transparent to your programs.

You should always use SSH keys, which was discussed in
[Lecture 2](./shell-intermediate.md#ssh-intermediate). Many cloud server providers allow you to provide the public SSH key when setting up a server. Unless you have a very good reason not to, I would recommend *disabling password login and root login* by editing `/etc/ssh/sshd_config` again:

```conf
#PermitRootLogin prohibit-password   # change to below
PermitRootLogin no                   # to disable root login via SSH

# ...

# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes  # change to below
PasswordAuthentication no    # to disable password authentication (only allowing SSH keypairs)
```

**Ensure that you can log in with the SSH key before doing this!**


#### Firewalls

Firewalls restrict or block certain network traffic based on *rules*. In Linux, this is done through packet filtering in the kernel and is configured through a standard tool, [iptables](https://en.wikipedia.org/wiki/Iptables).

Managing firewalls and networking is a topic of its own, though I would recommend reading [Networking for System Administrators](https://archive.org/details/networking-for-systems-administrators-michael-w-lucas) if you are interested. A tool exists to simplify the use of iptables: [Uncomplicated FireWall (UFW)](https://wiki.ubuntu.com/UncomplicatedFirewall).

UFW allows you to configure iptables via a (relatively) simple frontend. The following commands (note the use of `sudo`) enable SSH access on port 22 and disable all other traffic:

```bash
sudo ufw default deny incoming   # denies incoming connections by default
sudo ufw default allow outgoing  # allows outgoing connections by default (usually what you want, unless you are in a heavily regulated environment or if you know which external requests your server needs to make)
sudo ufw allow ssh/tcp   # allows port 22 - SSH port
sudo ufw logging on      # enables logging
sudo ufw enable          # enables firewall and applies the rules
sudo ufw status          # displays status of the firewall
```

There are numerous examples of UFW configurations available online. If you are using Docker and want to expose containers to traffic, I recommend the [ufw-docker](https://github.com/chaifeng/ufw-docker) setup.

Another useful tool for a single-node server is [Fail2Ban](https://github.com/fail2ban/fail2ban). This tool monitors server access logs and restricts malicious traffic by setting iptables rules. It can be [configured to protect SSH](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-20-04) as well as other common services.


#### Hosting Applications

You have already learned about [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/) in [Lecture 8](./virtualization.md).

It is a powerful tool for deploying applications on single-node servers. In order for these applications to be accessible to the outside world, they need to have *ports exposed on the host* which, as we discussed earlier, increases the risk from malicious actors.

One way to mitigate this risk (and get TLS encryption for free) is to use a [reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy). With docker-compose, it is straightforward to set one up for your application using the [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy).

When the container is running, any container that has certain environment variables set will have a configuration entry generated for [NGINX](https://nginx.org/en/) allowing it to be reached by web traffic on ports 80 and 443. A simple setup might look something like this:

```yaml
services:
  nginx-proxy:
    image: nginxproxy/nginx-proxy:latest  # choose a specific tag for production
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro  # NGINX-proxy uses the Docker UNIX Socket to monitor running containers
    environment:
      HTTPS_METHOD: nohttps


  app:
    image: myapp  # this can be any image which exposes a HTTP port
    environment:
      VIRTUAL_HOST: "myapp.example.com"  # set to the IP address or custom domain name of your server
      VIRTUAL_PORT: "8080"  # set to the port exposed by your application
```

In the above example, connections to `http://myapp.example.com` will be proxied to the `app` container.

You should always [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) to secure connections for any service you deploy on the public internet. This ensures that connections to the server are encrypted with HTTPS.

The [acme-companion](https://github.com/nginx-proxy/acme-companion) container can be deployed alongside NGINX-proxy to handle renewal of TLS certificates. For this to work, you will need to have a [domain name registered](https://en.wikipedia.org/wiki/Domain_name_registration) - you can buy domain names from providers such as [Gandi](https://www.gandi.net/en-US) for around £10 per year.

ACME-companion handles the issue and renewal of TLS certificates that *resolve to the host* running the containers. A minimal working example might look like this, given that we have done the following:

- Registered the `myapp.example.com` domain
- Set up an `A` or `AAAA` record which points `myapp.example.com` to the IP address of our server
- Allowed TCP IPv4 & IPv6 traffic access through ports 80 and 443 on our firewall
- Configured UFW to handle Docker traffic

```yaml
services:
  nginx-proxy:
    image: nginxproxy/nginx-proxy:latest  # choose a specific tag for production
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro  # NGINX-proxy uses the Docker UNIX Socket to monitor running containers
      - html:/usr/share/nginx/html  # this is to serve the ACME challenge
      - certs:/etc/nginx/certs:ro   # this provides the TLS certificates to NGINX to handle SSL termination
    network_mode: bridge

  acme-companion:
    image: nginxproxy/acme-companion:latest  # choose a specific tag for production
    container_name: nginx-proxy-acme
    environment:
      DEFAULT_EMAIL: "myname@example.com"  # set to the email address that you have used to register the domain
    volumes_from:
      - nginx-proxy
    volumes:
      - certs:/etc/nginx/certs:rw  # this stores TLS certificates when they are updated
      - acme:/etc/acme.sh  #
      # TODO check configuration

  app:
    image: myapp  # this can be any image which exposes a HTTP port
    environment:
      VIRTUAL_HOST: "myapp.example.com"  # set to the IP address or custom domain name of your server
      VIRTUAL_PORT: "8080"  # set to the port exposed by your application
```

It is also worth thinking early on about how to *back up your application*. There are countless ways of doing this, and the simplest may be to use the functionality provided by your server provider. For example, Digital Ocean provide the ability to [back up full-disk images](https://docs.digitalocean.com/support/how-do-i-manually-back-up-my-droplet/) for additional cost. However, this means it is not simple to migrate to a different server provider.

Many applications e.g. [MariaDB](https://en.wikipedia.org/wiki/Mariadb) can be directly backed up by compressing the contents of a specific directory to a tarball using the `tar` command. This is especially handy when combined with Docker, as the contents of the backup can be restored simply by unarchiving the contents of the file and mounting the directory to the appropriate location on the container. This is more flexible as it allows the data to be easily transferred to a new provider.

Make sure to *test your backup restoration process*! It is very easy to do this now with Docker, so there really isn't an excuse.


### Server Management (Again)

As applications and organisations scale, managing individual servers can take up a significant amount of time and resources.

A common analogy that you might hear is [cattle, not pets](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets). When you are managing a couple of servers it often feels like you are 'taking care of them' - ensuring they are kept up to date with package updates, checking up on them etc. These servers are often *indispensable* as the loss of the server would mean the loss of the application.

Modern cloud-based software engineering treats servers as standardised, low-cost, replaceable nodes. This is frequently abstracted away by Cloud providers (see sections below on [cloud managed services](#cloud-provider-managed-services) and [cloud automation](#cloud-automation-tools)) but you can achieve the same effect using *server configuration management tools* such as [Ansible](https://en.wikipedia.org/wiki/Ansible_(software)), [Puppet](https://en.wikipedia.org/wiki/Puppet_(software)) or [Chef](https://en.wikipedia.org/wiki/Progress_Chef).

I would recommend getting started with [Ansible](https://docs.ansible.com/) as it is widely used and open-source. Even if you only manage several servers, it can be used to streamline common tasks such as system updates and service configuration.


## Version Control in Industry


<div class="note">
Refer to <a href="./version-control">Lecture 3</a> and <a href="./open_source_dev_with_git">Lecture 7</a> for an introduction to Git and using it for open source development.
</div>

All of what you have learned about [open source development with Git](./open_source_dev_with_git.md) also applies in industry. It is very possible that you will need to work with other Git hosting services such as an [industry-managed GitLab instance](https://docs.gitlab.com/ee/topics/offline/quick_start_guide.html), [Azure DevOps](https://azure.microsoft.com/en-us/products/devops/) or any "enterprise-y" providers, depending on your workplace, but it is equally likely that you will be using GitHub.

There are a few Git-specific things that I recommend getting used to:

- [Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing), re-applying changes made on a branch on top of another branch;
- [Tags and Releases](https://git-scm.com/book/en/v2/Git-Basics-Tagging), annotations on specific commits indicating specific importance
- [Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks), especially the pre-commit hook


### Branch Discipline

When you are working on your own projects, you maybe used to committing everything to `main` or `master`.

As a project grows in size and complexity it becomes impossible to manage multiple changes to a single branch, especially with more than one developer. This is where *branch discipline* comes in.

[Branches](./version-control.md#branches) allow parallel work to be done e.g. making a new feature and fixing a bug could be two different branches. The goal should always be *eventually merge branches into main* and *not lose track of work in branches*.

Many developers get confused between branches and [Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests). The former is a core feature of Git and the latter is a bolt-on from GitHub. Pull requests are a really useful way of keeping track of changes that are made on a branch when you are working on many different branches simultaneously, as they can be associated with a ticket (e.g. a [GitHub Issue](https://docs.github.com/en/issues/tracking-your-work-with-issues)) and will be clearly visible in the [Pull Requests](https://github.com/afnom/missing-semester/pulls) for the repository so others can see them. *If you are going to be working on a branch for a long time, I recommend putting it in a pull request early on*.

There is debate about how to manage branches (branching strategies; see [discussion](https://stackoverflow.com/questions/39585900/what-is-the-difference-between-develop-vs-feature-branch-type)). This can broadly be summarised as a choice between:

- Always having a `main` branch that is in a deployable state
- Having separate branches for development, features, releases, hotfixes etc.


This really depends on what your project is and how it is deployed or released. For some work projects you may have only one or two environments, in which case maintaining more than one deployable branch is a lot of extra work. Other projects may be deployed in 4+ environments (dev, test, qa, preprod, prod etc.) and may diverge significantly at any one time. For open source projects e.g. Python, it isn't uncommon to have several [currently maintained releases](https://www.python.org/downloads/) which all have commits added to them! *Pick whichever strategy works best for you and your team*.


### Handling Pull Requests

Whether you are working in a junior or senior role, it is important that everyone knows how to deal with pull requests (or whatever equivalent term your system uses).

There are several things that make this process much easier for everyone:

- Making sure that *your branch is rebased on top of the branch you are merging onto*
- Making sure that *your branch has been squashed to remove small commits* e.g. fixing a typo or changing a variable name
- Making sure that *your branch uses descriptive commit messages*
- Making sure that *your branch has passed pre-commit hooks*
- Making sure that *you have covered everything in the checklist*
- Making sure that *your branch has been reviewed*


Rebasing is one of the most powerful features of Git and it is sadly underused and misunderstood. In my opinion, the best way of using it is within [Emacs](./emacs.org) using the [Magit](https://magit.vc/) package. Rebasing can get you out of a lot of trouble that you may find yourself in when using Git. (for anything else, there's [git-reflog](https://git-scm.com/docs/git-reflog)!) By rebasing your branch when your PR is ready it ensures that all conflicts are dealt with.

Adding lots of commits at once from a branch pollutes the history. You can [use Magit to clean up small commits](https://systemcrafters.net/mastering-git-with-magit/using-interactive-rebase/) via interactive rebasing, which is helpful for combining tiny changes together. In general, *each commit should be a small, easily-reversible change*. Ideally a PR should be a single commit, but this isn't always a good idea. You can also choose to [squash commits on merge](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/configuring-commit-squashing-for-pull-requests) instead of making a merge commit, but this results in losing information that may be present in the commit message that could be valuable to someone looking through the history later.

Related to this is the need for *descriptive commit messages*. If you are in control of a project, I recommend reading the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) spec and sticking to it. If you are contributing to another project, follow the style that they use - it is better to keep everything consistent, even if you lose out on a standardised approach like Conventional Commits. It is much easier to write descriptive commit messages if you have are making *small, idempotent changes*.

Another really useful way of standardising Pull Requests is to use pre-commit hooks. These are programs that are run before any commit which can prevent a commit from taking place before they have all run successfully. I recommend installing [pre-commit](https://pre-commit.com/) and setting up a `.pre-commit-config.yaml` file with standard hooks. There are endless possibilities e.g.:

- Removing trailing whitespace
- Preventing secrets from being committed
- Preventing code from being comitted with syntax errors
- Preventing code from being committed with merge conflict markers (`<<<<<<< HEAD` etc.)
- Many more possibilities for whichever tools you are using


It is possible to [configure templates for pull request](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository) which could include a checklist to make sure that the author of the PR has completed necessary tasks. Many open source projects use this to simplify the burden of reviewing PRs by ensuring that they all have completed basic tasks e.g.:

- All tests running successfully
- Relevant updates made to documentation
- Documentation generated successfully
- [CI pipelines completed successfuly](#continuous-integration-and-delivery)

Finally, it comes to the review. This is the time for a human ([or a machine](https://github.com/marketplace/actions/ai-code-review-action)) to give feedback on the changes that are proposed. Many developers hate reviewing Pull Requests or waiting for their PRs to be reviewed, but it is a valuable step to ensure that changes being made are visible and that the author has *thought about the wider impact of their changes*. It is **not** a good place to nitpick about indentation, variable naming etc. These should ideally be dealt with before it gets to the review stage, and having a linter and autoformatter (e.g. [Ruff](https://docs.astral.sh/ruff/) for Python) running in pre-commit hooks is a great way to reduce the noise.


### Releases

How (and when) you decide to publish a release is up to you and your team.

It is possible to configure [continuous integration pipelines](#continuous-integration-and-delivery) to generate a
[GitHub release](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases) on every push to `main` in a repository, which may be what you want if you have `main` as the deployable branch.

It is worth thinking about how you should name releases, and there are two main options:

- Semantic Versioning or *SemVer* (e.g. `v<major>.<minor>.<patch>`)
- Calendar Versioning or *CalVer* (e.g. `v<year>.<month>.<date>`)

I personally use both - I prefer SemVer for packages and libraries, where the major / minor / patch version distinction is quite useful, and CalVer for configuration updates. Some projects use flat version numbers with no distinction between major or minor versions - use whichever you feel is most appropriate.

Depending on what sort of software you are creating, a release may be the best way to alert your users to new features or bugfixes. It is important to *document the release in non-technical language* if possible.


## Continuous Integration and Delivery

<div class="note">
Refer to <a href="./open_source_dev_with_git#6-cicd-and-github-actions">Lecture 7</a> for more background on CI using GitHub.
</div>


As software projects scale it becomes increasingly important to check the quality of code that is being committed. Once validated, we may want to automatically deploy it to our uses. This is where Continuous Integration (CI) and Continuous Delivery (CD) enter the picture!

### Continuous Integration (CI)

You may already be used to running tests, linters etc. in your local repository (see [CLI Tools](#cli-tools-and-text-editors) and [Pre-Commit Hooks](#handling-pull-requests)). While this works well when you have a personal project used by very few people, it does not scale well when you have hundreds of commits and dozens of releases happening a week - it is very easy to forget to run the command and risk pushing code which has significant bugs!

**CI** automates the process of running common tasks as part of the test and build process. It may include:

- Running *unit tests*, *integration tests*, *end-to-end tests* etc. (to test for any regression in functionality)
- Running *linters* and *auto-formatters* (to check for common language mistakes and ensure consistent code format)
- Running *static analysis* tools (to validate behaviour without running code e.g. Python type checking with [MyPy](https://mypy.readthedocs.io/en/stable/getting_started.html) [^2])
- Extracting *documentation* from source code (to ensure published documentation is up-to-date with source code updates)
- Generation of *build artefacts* (to ensure that the code is transformed into the desired format, [see below](#build-artefacts)
- Creation of *OCI container images* (to allow the code to be deployed into a runtime environment such as [Docker](./virtualization.md))


There are countless examples of tasks that you might decide to run, and it will depends on what your project is and how it is developed and deployed. For example, see the [SQLite3 testing](https://www.sqlite.org/testing.html) page for how an extremely widely deployed library gets tested. For your projects, it may be sufficient to just run unit tests and generate a package artefact.

#### How CI is run

Each CI run is typically configured as part of a **pipeline** (e.g. [GitLab](https://about.gitlab.com/topics/ci-cd/cicd-pipeline/)). However, there is no standard way of configuring pipelines across CI providers, though many rely on [YAML](https://en.wikipedia.org/wiki/YAML) files (with all of the [issues](#yaml-hell) this causes!).

Under the hood, any CI provider will execute pipeline tasks in a shell. It will make maintaining and debugging your pipelines much easier if you *use locally-runnable components as much as possible* - stick to easily composable tasks that can be reproduced locally. I've had some success doing this in [GitHub Actions](https://github.com/features/actions) using the [Just](https://github.com/casey/just) command runner. However, you will almost always need to use some CI provider boilerplate to setup the environment and install dependencies.

Similarly to [cloud providers](#cloud-provider-managed-services), you have to be wary of *vendor lock-in* when setting up CI. The more you rely on features from a specific CI provider, the harder it will be to migrate providers in the future. By applying the core principles taught in this cause, you can set up your automations so they are as repeatable as possible which will ease any future rework or migration.


#### CI Providers

We mentioned [GitHub Actions](https://github.com/features/actions) in our [previous lecture on open-source development](./open_source_dev_with_git.md). This is a relatively simple way to get started with CI and CD and it is *free for public repositories*.

Most CI providers *bill by the minute* for tasks that they run. This is usually rounded up, so a task that fails within 5 seconds will be billed as a 1 minute task.

I have found the free tier of GitHub actions to be sufficient for the volume of pipelines that we run, even with the [limits](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions#included-storage-and-minutes) (2000 minutes per month) that GitHub impose.

Most providers (including GitHub and GitLab) allow use of *self-hosted runners*. These can typically be run as a Docker container that connects to the CI provider's API and receives requests to run jobs. These do *not incur CI a CI minutes quota*, but you are responsible for configuring and maintaining the machines running the runners. If you have spare hardware or a [Kubernetes cluster](#kubernetes), this can give significant savings and allow running more pipelines.

There are [many other providers](https://en.wikipedia.org/wiki/Comparison_of_continuous_integration_software) of CI services. In many organisations, you won't have any control over which provider you use e.g. heavy users of Microsoft products may mandate use of [Azure DevOps Services](https://azure.microsoft.com/en-us/products/devops/). Popular CI providers such as [CircleCI](https://circleci.com/), [Jenkins](https://www.jenkins.io/) and [Travis CI](https://www.travis-ci.com/).

Regardless of provider, you will want to consider the following:

- Options for *self-hosting* the provider (some e.g. [Jenkins](https://www.jenkins.io/) are open-source and containerised)
- Integration with *version control* (most providers have an integration with e.g. GitHub)
- Cost!


#### Build Artefacts

The key outputs from CI are *build artefacts*. These could be tarballs of the source code, compiled binary executables or libraries, software packages in different package manager formats, OCI container images, or anything else you can imagine.

We talked briefly about [GitHub releases](#releases) earlier. These are often generated automatically by CI pipelines to include artefacts generated for that specific Git tag. If you have ever downloaded a binary from GitHub for a given version of the package, that binary will likely have been generated using a pipeline and stored as an artefact from that pipeline.

Be aware of the **security implications** of software delivery artefacts! It is very important that your CI pipelines are correctly configured and secured and that you verify any outputs that are generated e.g. by verifying [GitHub artefact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds), [signing binaries](https://joemiller.me/2019/07/signing-releases-with-a-gpg-project-key/) etc. Be sure to protect the build process itself from [poisoned pipeline execution](https://owasp.org/www-project-top-10-ci-cd-security-risks/CICD-SEC-04-Poisoned-Pipeline-Execution), as a compromised CI pipeline is a significant risk to your project and those who depend on it.


#### Validation Stages

Another use case for pipelines is in ensuring that any changed proposed to a project pass *quality gates*. This helps in streamlining *code reviews* (see [Handling Pull Requests](#handling-pull-requests)), whereby a project maintainer can refuse to review a patch unless it has e.g. full coverage by unit tests.

Pipelines can also be run conditionally. You may wish to only generate artefacts for code that is pushed to `main`, or has been explicitly tagged as a release. When maintaining [multiple branches](#branch-discipline) and multiple environments, you can make clever use of pipelines to ensure that required checks are run and, in combination with [Continuous Delivery](#continuous-delivery-cd), that the artefacts get deployed in the right places with the right configuration.


### Continuous Delivery (CD)

Following the generation of artefacts, your operations team will need to deploy them to live environments. While often a manual task, the automation of this using **CD** can be used to ensure that changes are deployed in a [repeatable and reliable manner](https://www.atlassian.com/continuous-delivery/principles).

Implementation of CD follows the same guidelines as that for [CI](#continuous-integration-ci) and may often run as part of the same CI tool.


#### How and When to Automate

`Fully hands-off' deployments are extremely tempting goals for DevOps teams. Depending on your project, it may (or may not) be desirable to continuously deploy any changes directly to users. This will affect which options you choose for achieving continuous delivery.

Before you start considering options, *document your existing deployment process in a runbook*. By writing down all of the tasks that you need to do to get code from your Git repo into production, you will have a reference document which describes the whole process and from which you can find low handing fruit for automation.

Always remember: automation of processes is great *until it goes wrong* and you have to diagnose the issue. This is why it's equally important to have a robust process to *roll back changes*.


#### Simple applications

For some applications, deployment is as simple as changing some environment variables and running `docker compose up -d`.

Running Ansible (see [Server Management](#server-management-again)) in a CD pipeline can be a very effective way of updating one or more servers with simple code changes. Ansible can even be configured on a remote system to *automatically pull changes* and apply them (see [ansible-pull](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html)), which can greatly simplify your deployment model.


#### Infrastructure-as-Code (IaC)

IaC tools such as [Terraform](#terraform-and-opentofu) and [Kubernetes](#kubernetes) greatly ease the burden of deploying changes to complex environments, as they are designed for *declaratively managing configurations* of infrastructure and applications. When run in CD pipelines they can be used for reporting planned changes for human approval (an additional *quality gate*) before the changes are applied to the live environment.

For more complex applications, IaC tools are very effective when combined with CD pipelines, but you have to very careful in *packaging changes incrementally* so that you can safely roll back if needed e.g. don't make simultaneous changes to application code at the same time as deploying an updated infrastructure architecture.


#### Final Thoughts

There are two trade-offs that I want to leave you with when thinking about CD:

- **Deploy Often** vs **Big Releases**
- **Deploy during off-periods** vs **Deploy while team is working**

Many projects make a big deal out of large releases with a lot of changes. When releasing directly to end users, you are better off ensuring that you can [make small, frequent changes](https://dafyddvaughan.uk/blog/2017/deliver-little-deliver-often-avoid-the-big-bang/). This agility will give you confidence in your processes and allow you to hone your operational capability.


Once your deployment (and rollback) strategy is battle-tested, you may elect to push deployments back to a quieter time (e.g. during the middle of the night, when all your users are asleep) to minimise disruption. In an ideal world, your users *should not even notice when an update is applied* or can elect to download the update at their convenience. The old adage `Don't deploy on Friday' is often repeated, but [with the right migitations](https://community.aws/content/2fmLHThOhoYEONmzGUFsx1qVKKd/deploy-on-friday-devops-best-practices) it should be no difference to deploying on any day of the week.


However, no one wants to get called in at 3am!


## Documentation and Project Management

### Documentation

TODO

- Documentation generators e.g. MkDocs
- Markdown as standard format vs Web-based tools e.g. Confluence
- Vendor lock-in concerns e.g. Atlassian vs text-based files
- Publishing automatically in CI pipelines
- Docstring extraction
- Standardisations
- Diagrams as Code e.g. PlantUML, MermaidJS
- Automated generation of e.g. ERD diagrams, system architecture diagrams, Software BOM


### Project Management

- Enforced use of tools e.g. Jira vs Organic use through e.g. GitHub Issues vs Spreadsheet-based
- Intent of Agile vs what it often ends up becoming in large industries
- Having big teams dedicated to e.g. platform, architecture, security vs Keeping teams skilled and independent
- Best practices for reporting on progress
- Iteration
- Constantly shipping



## LaTeX for Industry/Academia

TODO

- Converting between markup languages
- Pandoc
- Beamer for presentations and posters
- Accessibility concerns
- HTML vs PDF


## Web Application Frontends

TODO

- Simplicity (HTML/CSS/JS) vs Complexity (Frameworks)
- 'Choose Boring Technology'
- SPA vs Static
- JavaScript vs TypeScript vs HTMX vs JSX


## Database Design

TODO

- Relational / Key-Value / Graph / Time-Series / ??
- Considerations to make
- Different providers
- Open-Source vs Proprietary
- 'Choose Boring Technology'


## Cloud Provider Managed Services

Increasingly, organisations are purchasing compute from *cloud providers* instead of maintaining their own physical servers.

This is *almost always more expensive* than self-hosting, and you may well choose to pick *free or cheap* options for your own projects, such as [GitHub](https://pages.github.com/) or [GitLab](https://docs.gitlab.com/ee/user/project/pages/) Pages (free static web hosting), [Hetzner](https://www.hetzner.com/) (cheap VPS offerings) or [Backblaze B2](https://www.backblaze.com/) (cheap blob storage compatible with S3 protocol). However, most reasons organisations choose cloud providers have little to do with price and more to do with [compliance](https://learn.microsoft.com/en-us/compliance/assurance/assurance-risk-assessment-guide), [support](https://aws.amazon.com/premiumsupport/) and [compatibility with their existing IT estate](https://learn.microsoft.com/en-us/entra/identity/domain-services/compare-identity-solutions).

In this section I'll talk about the following cloud providers, though there are many more that you can try:

- Amazon Web Services ([AWS](https://aws.amazon.com))
- Microsoft [Azure](https://portal.azure.com/)
- Google [Cloud](https://cloud.google.com/) (GCloud)
- Oracle [Cloud Infrastructure](https://www.oracle.com/cloud/) (OCI)
- [Hetzner](https://www.hetzner.com/cloud/)
- [DigitalOcean](https://digitalocean.com/)
- [Cloudflare](https://www.cloudflare.com/en-gb/)


### Standard Offerings

Most cloud providers will provide the following:

- Some form of *Virtual Private Server* ([AWS EC2](https://aws.amazon.com/ec2/), [Azure Virtual Machines](https://azure.microsoft.com/en-us/products/virtual-machines/), [DigitalOcean Droplet](https://www.digitalocean.com/products/droplets), [GCloud VPS](https://cloud.google.com/learn/what-is-a-virtual-private-server), [Hetzner VPS](https://www.hetzner.com/), [Oracle VM](https://www.oracle.com/cloud/compute/virtual-machines/))
- Some sort of *private networking* ([AWS VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), [Azure VNET](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview), [Digital Ocean VPC](https://docs.digitalocean.com/products/networking/), [GCloud VPC](https://cloud.google.com/vpc), [Hetzner Network](https://docs.hetzner.com/cloud/networks/getting-started/creating-a-network/) , [Oracle VCN](https://www.oracle.com/cloud/networking/virtual-cloud-network/))
- A managed *block storage* for VPS filesystems ([AWS EBS](https://aws.amazon.com/ebs/), [Azure Disk Storage](https://azure.microsoft.com/en-us/products/storage/disks/), [DigitalOcean ??](), [GCloud ??](), [Hetzner ??](), [Oracle ??]())

These all mirror the networking, compute and filesystem configurations that you could expect from an on-premises data centre (though there is significant variability between providers, e.g. [Azure VNET compared with AWS VPC](https://devblogs.microsoft.com/premier-developer/differentiating-between-azure-virtual-network-vnet-and-aws-virtual-private-cloud-vpc/)). *Be sure to follow best-practices for whichever cloud provider you are using*, as it will save you headaches in the future!

These services may well be sufficient for your applications, but there are some other managed services that you may want to consider.


### Blob Storage

On Linux and UNIX machines, you may be used to *block storage* as the basis for filesystems (accessed through commands like `lsblk`, `blkid` and `mount`.

Many cloud providers offer *blob storage* as a service. The most well-known example of this is [AWS S3](https://aws.amazon.com/s3/). Blob storage allows applications to store and retrieve any kind of data over a standard API. It is an excellent choice for backups and for hosting static websites, though frequently accessed, low-latency data should still be stored on a filesystem.

Blob storage providers offer other features, such as *automatic expiration*, cycling to *lower-cost longer term storage*, *versioning* and *encryption-at-rest*. Amazon offers an insane *11 nines* (99.999999999%) of durability guarantee for data stored in blob storage due to the amount of redundancy they employ, yet it is still very low-cost for storing gigabytes or terabytes of data.

Other cloud providers offer similar services:

- [Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs/)
- [Google Cloud Storage](https://cloud.google.com/storage)
- [DigitalOcean Spaces](https://www.digitalocean.com/products/spaces)
- [Backblaze B2](https://www.backblaze.com/cloud-storage)
- [Hetzner Object Storage](https://www.hetzner.com/storage/object-storage/)
- [OCI Object Storage](https://www.oracle.com/cloud/storage/object-storage/)
- [Cloudflare R2](https://www.cloudflare.com/en-gb/learning/cloud/what-is-blob-storage/)

Again, be aware of possible *vendor lock-in* when integrating closely with managed blob storage services, though many provide an S3-compatible API. The biggest factor in migration from one provider is likely to be fees for exporting or importing data.


### Content Delivery

A useful managed service which integrates well with blob storage is a *content delivery network* (CDN). It is fundamentally a distributed set of servers spread across many data centres in different geographical locations, which speeds up access to content that it *fronts* e.g. a static website.

While building your own CDN is very difficult and expensive, cloud providers offer these as services. Here are some examples available from popular cloud providers:

- [AWS Cloudfront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
- [Cloudflare CDN](https://www.cloudflare.com/en-gb/application-services/products/cdn/)
- [Azure Front Door](https://azure.microsoft.com/en-us/products/cdn/)
- [Google Cloud CDN](https://cloud.google.com/cdn)



Another significant advantage of a CDN is the protection that it can give you from *malicious attacks*, with support for *validating HTTP requests*, *load balancing* and *origin failover*. It can be combined with a *firewall* for further protection of your network. You can leverage a CDN even if you aren't using cloud services, and it can ultimately reduce costs especially if your cloud provider *charges for egress*.


### Managed Databases

Many of the types of database discussed in [database design](#database-design) are offered directly as managed services.

Cloud providers frequently offer multiple different RDBMS instances e.g. AWS Relational Database Service offers [PostgreSQL](https://aws.amazon.com/rds/postgresql/) and [MariadDB](https://aws.amazon.com/rds/mariadb/) along with MySQL, Oracle and Db2. Some even offer *severless instances* e.g. [Aurora PostgreSQL](https://aws.amazon.com/rds/aurora/) which can free you from the limitations of a single-node database but potentially incur higher costs based on your use-cases.

If you are using an RDBMS, another consideration you may make are the *expected workloads* that you are running. A transaction-based workload is better suited to e.g. AWS RDS whereas an analytical workload would run better on AWS Redshift.

Managed databases are often worth the extra cost as it removes much of the burden of administrating, updating and backing up the database. By relying on an open-source engine such as PostgreSQL, you can *reduce impact of migrations* as it will most likely work on a different managed service or on-premises instance with minimal code changes. However, be aware that *extensions* can be difficult or impossible to install on managed database services.


### Serverless Compute

If you have written code that needs to run very infrequently or handle very few requests, you may wish to avoid paying for an expensive always-on VPS.

Different providers give different names to this service e.g. [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html), [Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), [GCloud Functions](https://cloud.google.com/functions). They are all functionally the same: you write code in a language e.g. Python and upload it to their service, and the code runs when it is triggered, which could be from:

- upload of a file to blob storage
- a web request being received
- on a schedule similar to Cron
- many other things!


They are worth considering depending on how frequently they will be called, and have some other useful features e.g. [integration with CDNs](https://aws.amazon.com/lambda/edge/).


### Container Runtimes

Many applications that you build will run in [containers](./virtualization.md), and most cloud providers offer ways to run containers without having to manage your own servers directly e.g.:

- [AWS Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- [Azure Container Apps](https://learn.microsoft.com/en-us/azure/container-apps/overview)


They generally aim to be simple and developer friendly, however are another example of *vendor lock-in*, as it is difficult to migrate directly to another cloud provider's service. To avoid this, I recommend using a managed Kubernetes service which provides almost identical APIs across different cloud providers. I'll discuss Kubernetes [towards the end of these notes](#kubernetes).


### Everything else

There's a lot more to cover on the many different services available from cloud providers. There are countless specialised services for everything from real-time gaming, to machine learning, to search and indexing.

I've introduced what I believe to be core, provider-agnostic services that you may wish to utilise in your work. Remember that *the more cloud services that are used, the harder a migration will be* - using *open-source cores* e.g. PostgreSQL and Kubernetes as much as possible simplifies any future migration immensely!



## Cloud Automation Tools

### CLI for Cloud

The "cloud" is a huge layer of complexity which supposedly aims to make complicated topics (networking, system administration, database administration, etc.) accessible to anyone with a web browser!

Fortunately, if you ever have to work with cloud provider services there are plenty of command line tools at your disposal:

- [AWS CLI](https://aws.amazon.com/cli/) (Amazon Web Services)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/) (Microsoft Azure)
- [gcloud CLI](https://cloud.google.com/sdk/docs/install) (Google Cloud)
- [doctl](https://docs.digitalocean.com/reference/doctl/) (Digital Ocean)
- [hcloud CLI](https://github.com/hetznercloud/cli) (Hetzner Cloud)
- [Oracle Cloud Infrastructure CLI](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cliconcepts.htm) (Oracle Cloud)

### JSON as an exchange format

Most of these tools output quite a lot of data and have generally standardised on [JSON](https://en.wikipedia.org/wiki/JSON) as an data exchange format. You may be familiar with *JavaScript Object Notation* if you have done any work with JavaScript in the past.

There are many tools available for working with JSON on the command-line. The most popular by far is [JQ](https://jqlang.github.io/jq/) and many scripts interacting with the output of cloud CLI tools will assume the use of it, however the syntax can be cumbersome and unintuitive.

<div class="note">
Large Language Models such as Claude or ChatGPT often provide good instruction on specific JQ syntax.
</div>

**Example**: Reporting on a fleet of AWS EC2 instances (virtual machines in Amazon Web Services).

```bash
# TODO
```


### Infrastructure as Code

While the individual CLI tools can be used to create and manage resources, it is still difficult to keep track of everything that is created and how it was configured. Ideally, we would be able to keep track of our infrastructure and control which changes are made and by whom. As we are already doing this with our code, we can leverage [Git](./version-control.md) to keep track of our cloud deployments - which also gives us collaboration and versioning for free!

Cloud infrastructure automation tends to work something like this:

1. Declare what you want your infrastructure to look like in a *text configuration format*
2. Validate your configuration against the live cloud environment to *list resources to be provisioned*
3. Commit to *provisioning the resources*, incurring costs on your cloud provider account
4. Store the *state of the provisioned resources* for later use
5. Any future changes are *compared with the current state* so that resources can be modified rather than deleted and re-created


Several cloud providers provide a tightly-integrated service which can do the above:

- [CloudFormation](https://aws.amazon.com/cloudformation/) (Amazon Web Services)
- [Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview) (Microsoft Azure)


However, these tools are tightly coupled to the cloud platforms that provide them. Another common theme in this talk is to **beware vendor lock-in**! While we may decide to commit to using a particular cloud provider, if we work in a vendor-agnostic configuration language we can potentially save ourselves headaches down the line if we have to migrate e.g. from Azure, to AWS, then back to Azure (*I have had to do exactly this*).

There are many different Infrastructure-as-Code tools available but I will be focusing on one - [Terraform](https://www.terraform.io/), created by [HashiCorp](https://www.hashicorp.com/).

### Terraform and OpenTofu

<div class="note">
Terraform has recently undergone a license change which, while it likely only affects organisations that are HashiCorp competitors, has resulted in the creation of a fork: [OpenTofu](https://opentofu.org/).

The tools have the same purpose and [migration to OpenTofu](https://opentofu.org/docs/intro/migration/) is generally straightforward, though they have diverged in features since the fork. If you are choosing between them for a new project, I would recommend reviewing the pros and cons of each tool before making a decision.

To avoid confusion, henceforth when I refer to Terraform and the `terraform` binary it applies to both Terraform and OpenTofu unless otherwise specified.
</div>

Terraform is a configuration language and application for *managing the state of resources*. A resource could mean a cloud server, a Docker container, a managed database service, or anything that you can think of which has a state! There are [over 30](https://registry.terraform.io/browse/providers) **official providers** listed on the official Hashicorp Terraform registry which cover major cloud platforms and there are numerous third-party providers as well which cover a multitude of cloud and local services.

One major advantage of Terraform is the configuration language used, [Hashicorp Configuration Language](https://developer.hashicorp.com/terraform/language) (HCL). Many cloud providers rely on either GUI-based or YAML/JSON-based configuration methods. By contrast, HCL is a fully-feature programming language with support for [functions](https://developer.hashicorp.com/terraform/language/functions), [input variables](https://developer.hashicorp.com/terraform/language/values/variables), [modules](https://developer.hashicorp.com/terraform/language/modules) and [dynamic / conditional blocks](https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks).

The following example defines an EC2 instance (a VPS in AWS) with a predefined Amazon Machine Image (AMI) set to Ubuntu 24.04 (see [AMI Image Locator](https://cloud-images.ubuntu.com/locator/ec2/) for an index of AMIs). [Many more parameters](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance) can be set depending on what configuration you wish to achieve, for example: deploying the instance in a specific subnet, attaching storage devices, or setting up monitoring.

```terraform
# Define an AWS EC2 instance
resource "aws_instance" "instance" {
  ami = "ami-091f18e98bc129c4e"  # 24.04 LTS in eu-west-2 on AMD64

  instance_type = "t2.micro"
}
```

If you are interested in trying out Terraform without paying for cloud resources, you can try out some communityproviders such as [kreuzwerker/docker](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs) for managing Docker container images and running containers, or [integrations/github](https://registry.terraform.io/providers/integrations/github/latest/docs) for managing GitHub repositories and CI pipelines.

Another valuable feature of Terraform for cloud automation is [data sources](https://developer.hashicorp.com/terraform/language/data-sources), which allows you to refer to resources defined elsewhere - either in your own Terraform configurations or from environments that aren't managed by Terraform. This can be very useful when your infrastructure becomes more complex! For example, you can pull configuration from your GitHub repository using the [integrations/github](https://registry.terraform.io/providers/integrations/github/latest/docs) provider:

```terraform
# Define the source repository and file
data "github_repository" "repo" {
  full_name = "afnom/missing-semester"
}

data "github_repository_file" "logo" {
  repository = data.github_repository.repo.name
  branch = "master"  # this could be changed to a specific tag or commit hash
  file = "favicon-32x32.png"
}

module "webapp" {
  # This module doesn't exist, but it could be implemented for e.g. configuring a webapp dynamically
  # by providing a configuration file or (in this case) a PNG logo

  source = "/path/to/webapp/module/"

  png_logo_base64 = b64encode(data.github_repository_file.log.content)
}
```



### Kubernetes

When you start thinking about using the Cloud, it will often be because the deployment options (e.g. [containers](./virtualization.md) using docker-compose) for your software have limitations. You may struggle with *managing individual servers*, even with automation tools such as Ansible, or may have *requirements on fault-tolerance*, such as keeping your systems running during data centre outages or server failure. You may wish to *scale your application* to handle increased demands from users during especially busy periods, or even *scale your entire infrastructure* to save money during periods of downtime.

While many tools are available from cloud providers which can allow you to achieve this, you may wish to *avoid vendor lock-in* and *choose open-source tools by default*. Fortunately, there is a cloud-native, open-source platform for orchestrating containers, and that is [Kubernetes](https://kubernetes.io/).

#### Why (or why not) use Kubernetes?

Kubernetes is *infamously complex*, which makes it difficult to approach. It is notorious for being used in **overengineering** unnecessarily complicated solutions where a simpler solution may be [just as effective and easier to maintain](https://betterprogramming.pub/why-we-over-engineer-software-and-how-to-break-the-habit-72a32b62e3e2?gi=99edbfc3f574), as well often featuring in *CV-driven development*, where a technology stack is chosen for the benefit of an engineer's career longevity rather than for a product's longevity. Furthermore, it is not a 'magic bullet' for cloud migrations!

<figure>
    <img src="https://assets-global.website-files.com/5d794055739ddf1e8da11999/5dd4242a86813427251ecc2f_oThDkrS-BI6o3-jMcXD7aAoUmC_f8Uzl9M0RR3t1khJcH-JCfUetjUGj-1bYzXw_f2wn9z6XIK7bbFXI8qdw4_Kv67gg1vX4ZCs9Sv0gn_rWkw63UEN4Ycce1VHCNAu7bl02fimo.png" alt="Dilbert Kubernetes cartoon © 2017 Scott Adams, Inc.">
    <figcaption>Dilbert comic strip satirising the understanding of cloud migration, containers and Kubernetes in management</figcaption>
</figure>

The complexity of Kubernetes is by design, allowing it to be (simultaneously) extremely flexible and adaptable as well as extremely standardised. There are innumerable options for running Kubernetes and you can use the same tool, [kubectl](https://kubernetes.io/docs/reference/kubectl/), to interact with all of them. You can even use Terraform to manage Kubernetes resources which I have done extensively and found numerous benefits, or use other tools built on Kubernetes such as [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift).


### Core Concepts

Docker has the concepts of **images** and **containers** (see the [lecture notes on virtualisation](./virtualization.md)). Terraform has the concepts of **resources** and **data sources**.

Kubernetes refers to different types of units that it manages as **kinds**. A [kind](https://kubernetes.io/docs/concepts/overview/working-with-objects/) is a type of resource that can be deployed on (and managed by) Kubernetes.

A given installation of Kubernetes is called a **cluster** and it consists of one or more **nodes**. A [Node](https://kubernetes.io/docs/concepts/architecture/nodes/) is a Kind in Kubernetes, just like all of the different Kinds discussed below! A locally deployed cluster will only have a single node, but clusters can be comprised of multiple nodes. A Node is a single physical or virtual machine, usually equivalent to a single instance of a VPS in a cloud provider e.g. EC2 instance in AWS or Droplet in DigitalOcean.

#### Running Containers

Like Docker, Kubernetes works with [OCI](https://opencontainers.org/) container images. The *minimum deployable unit* for a container image is called a [Pod](https://kubernetes.io/docs/concepts/workloads/pods/). Unlike Docker, the minimum deployable unit can include one or more containers [^1].

It is unusual to declare Pods directly. For most applications, you will work with Kinds that can be used to manage one or more pods: Deployments, Stateful Sets, Daemon Sets and (Cron) Jobs.

[Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) are where the scaling features of Kubernetes begin to appear. They allow a *desired number of pods* to be declared and managed automatically. This should generally be considered as the **default option** for deploying a container, as it can be scaled to one or more instances of the container both manually and [automatically](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/), and can be scaled down to zero to switch it off.

When a persistent state is required, you can use [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/). These are similar to Deployments in that they both manage a group of pods, but these are designed for *persisting data*. If you deploy databases or other applications that require persistent storage onto Kubernetes, you will typically configure them as StatefulSets. Do note that it is *considered best-practice to deploy stateful resources outside of Kubernetes* (e.g. using Cloud Managed Services) though there may be good reasons to ignore this: cost, simplicity of deployment (e.g. for local development environments) or lack of support for a particular service.

The other Kinds are more specialised. [DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ensure that *a pod runs on every node in the cluster*. This is essential for many system services required for the operation of the Kubernetes cluster. The first DaemonSet I configured myself was [NGINX](https://github.com/kubernetes/ingress-nginx) for exposing containers to the network - it must run on every node in order to route to containers on that node.

[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/) are Pods which are used to run one-off tasks to completion. They can handle failure, restarting until the task is completed. [CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) behave identically to Linux `cron` jobs and are used to schedule regularly occurring jobs such as batch data ingestion or backups.


#### Communicating with Containers

The Kinds discussed previously all relate to running containers. Many applications require containers to *talk to each other* e.g. a Java application may need to communicate with a MySQL database.

The primary way to make a pod reachable is to configure a [Service](https://kubernetes.io/docs/concepts/services-networking/service/). A Service provides a *cluster-specific network address* which can be used to reach the pods in a given grouping (usually a single Deployment, DaemonSet or StatefulSet). There are various different types - for most uses, you will configure the service as `ClusterIP`.

Services can expose different port numbers on target pods. For example, one webapp container may expose port 8080 for web traffic and another may expose port 5000. The two Services defined for these webapp containers can both expose port 80 (the standard HTTP port) in both cases, simplifying the configuration for applications which talk to these containers.

All Kinds discussed so far (apart from cluster-wide objects such as Nodes) can be **namespaced**. A [Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) is a logical isolation of resources (pods, deployments, services etc.) within a cluster. This can be used for separating environments from one another e.g. different teams, different applications or different stages (development, testing). By default, resources are created in the `default` namespace. It is recommended to *not use the default namespace*.

By combining namespaces with services, it is possible to address resources anywhere in the cluster. For example, a service named `my-app` in the `my-namespace` namespace can be resolved (within the cluster) from the following domain name: `my-app.namespace.svc.cluster.local`. Kubernetes distributions typically deploy DNS providers such as [CoreDNS](https://kubernetes.io/docs/tasks/administer-cluster/coredns/) as part of the installation process, which allows containers to resolve internal domain names.

In order to reach a Service or its target Pods(s) from outside the cluster, it must be **exposed** in some way. There are three main options:

- *Port-forwarding* (exposing a port on a host running `kubectl`, typically used for local or development environments)
- Configuring a Service of Type *LoadBalancer* (requires a cloud provider which can automatically provision Load Balancers e.g. [AWS](https://docs.aws.amazon.com/eks/latest/userguide/auto-configure-nlb.html), [Azure](https://learn.microsoft.com/en-us/azure/aks/load-balancer-standard))
- Configuring an *Ingress* (requires an Ingress Controller DaemonSet e.g. [NGINX](https://kubernetes.github.io/ingress-nginx/) to reverse-proxy traffic into the cluster)


This is where your configuration will vary significantly depending on the rest of your architecture. Many Kubernetes distributions e.g. AWS [EKS](https://aws.amazon.com/eks/) and Azure [AKS](https://learn.microsoft.com/en-us/azure/aks/what-is-aks) integrate tightly with other managed offering such as load balancers, API gateways, enterprise firewalls and content delivery networks. A significant amount of bespoke configuration can be necessary to take full advantage of these services.

It is possible to expose your Services to the outside world in a vendor-agnostic way using [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) resources. These require an Ingress Controller to be deployed on the cluster as a DaemonSet, and allow *networking rules* to be configured which expore Services on given URLs and paths. This can be combined with cluster-wide addons such as [cert-manager](https://cert-manager.io/) for automatically provisioning TLS certificates and [OAuth2-Proxy](https://github.com/oauth2-proxy/oauth2-proxy) for authenticating users against OIDC-compliant providers such as Google or Microsoft Entra ID.

For local development it is often simpler to *ignore ingress configuration* and directly **expose** ports on containers using port-forwarding, which we'll touch on in a later section.


### Choosing a Distribution

There are numerous Kubernetes distributions available. Most of the Kinds we have already discussed should work on any distribution, and you are most likely to run into issues when dealing with PersistentVolumes and Ingresses. Here is a (non-exhaustive) list of Kubernetes distributions:

- [Minikube](https://minikube.sigs.k8s.io/docs/) is a local Kubernetes distribution targeting Windows, macOS and Linux
- [Docker-Desktop](https://docs.docker.com/desktop/features/kubernetes/) includes a Kubernetes environment suitagble for local testing on Windows, macOS or Linux
- Kubernetes in Docker ([KinD](https://kind.sigs.k8s.io/)) is another local Kubernetes environment supporting all three OS'es
- [K3s](https://k3s.io/) is a lightweight Kubernetes distribution intended for resource-constrained environments
- [Kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/) is the official tool for creating Kubernetes clusters
- Azure [AKS](https://learn.microsoft.com/en-us/azure/aks/what-is-aks) is Microsoft Azure's managed Kubernetes offering
- AWS [EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) is Amazon Web Services' managed Kubernetes offering
- [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine) is Google Cloud's managed Kubernetes offering
- Digital Ocean [Managed Kubernetes](https://www.digitalocean.com/products/kubernetes)
- Other offerings from [Oracle](https://www.oracle.com/cloud/cloud-native/kubernetes-engine/), [Linode](https://www.linode.com/products/kubernetes/), [IBM](https://www.ibm.com/products/kubernetes-service) and [Alibaba](https://www.alibabacloud.com/en/product/kubernetes?_p_lc=1)


If you are using Linux, I would recommend using [K3s](https://k3s.io/) as it is simple to set up and extremely lightweight. If you are in a team using Windows and macOS, I would recommend [minikube](https://docs.docker.com/desktop/features/kubernetes/). Otherwise, I would use whichever managed Kubernetes service your cloud provider offers.

### Packages (Helm + Arkade)

Kubernetes has a package manager, [Helm](https://helm.sh/), which can be used to install applications and manage version upgrades. It is a handy way of *installing stateful applications*, as it automatically creates all of the different kinds of resources e.g. StatefulSets, Services, Secrets, Ingresses based on configuration in a *values* file.

Packages are provided as *charts* (yet another piece of nautical terminology!). You can download charts from numerous different repositories provided by organisations like [Bitnami](https://bitnami.com/).

For example, installing [PostgreSQL](https://github.com/bitnami/charts/blob/main/bitnami/postgresql/README.md) using a Bitnami chart:

```bash
helm install my-postgres oci://registry-1.docker.io/bitnamicharts/postgresql
```

*Be aware of the tradeoffs when installing stateful application on Kubernetes*, as management of [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) is extremely distribution-specific. You will likely find it easier to install Helm charts on cloud-managed K3s distributions which have default storage providers than to install them on a local Kubernetes distribution where you have to configure the storage providers yourself. In my experience, this is one of the biggest learning curves of Kubernetes.

A useful tool for getting started with Kubernetes-native tools and applications is [arkade](https://github.com/alexellis/arkade). It allows you to quickly install Kubernetes distributions and CLI tools on your machine / in CI as well as installing applications onto your Kubernetes cluster(s).

There are many useful applications that can be installed on Kubernetes - here is a few that I've found useful:

- [cert-manager](https://cert-manager.io/) for automatically provisioning TLS certificates from LetsEncrypt
- [Grafana](https://github.com/grafana/grafana) for viewing monitoring and observability dashboards
- [ingress-nginx](https://github.com/kubernetes/ingress-nginx) for provisioning Ingress resources using NGINX as a reverse proxy
- [inlets-operator](https://github.com/inlets/inlets-operator) for exposing local K3s distros (behind NAT) over public cloud Load Balancers
- [Prometheus](https://prometheus.io/docs/introduction/overview/) for collecting metrics from instrumented application code


### YAML Hell

Like many cloud-native tools, Kubernetes is primarily configured using [YAML](https://yaml.org/), an indentation-based markup language. As many of you may have found with Python, indentation makes it very difficult to automatically format code, as the indentation is important for control flow.

Furthermore, tools like Helm rely on *templated YAML* whereby the file is further complicated by adding lots of `{{ }}` syntax.

There are many ways of declaring Kubernetes resources without having to write YAML. I have personally found success using [Terraform](#terraform-and-opentofu) which has [Kubernetes](https://registry.terraform.io/providers/hashicorp/kubernetes/latest) and [Helm](https://registry.terraform.io/providers/hashicorp/helm/latest/docs) providers. This means that you can standardise your infrastructure and application code with [linters](https://github.com/antonbabenko/pre-commit-terraform) and [vulnerability scanners](https://github.com/bridgecrewio/checkov).


## Extra Sections?

### Observability

[^1]: When getting started with Kubernetes, you will likely only be deploying pods with a single container. You may wish to use multiple containers for e.g. initialising some shared resources needed by a container (see [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/), or for exporting metrics or other data from a running container (see [sidecar containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/).

[^2]: For some compiled languages e.g. [Rust](https://www.rust-lang.org/), the compilation process is a form of static analysis, whereas interpreted language such as Python need separate tools for this.
