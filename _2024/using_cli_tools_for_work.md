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

TODO

- 'Allow teams freedom of choice'
- Focus on tools, not editors
- Standard, easily-installable commands for common project tasks
- Standard configuration e.g. [.vscode](https://code.visualstudio.com/docs/getstarted/settings), emacs, vim shareable with team members
- Stick to text editor / IDE - keep documentation in plain text format


## SSH for System Administration

TODO

- Discipline in managing servers
- Useful tools (firewalls and networking - `NGINX`, `LetsEncrypt`, `Fail2Ban`, `ufw`, [containers](./virtualization.md), unattended upgrades etc.)
- [SSH Keys](./shell-intermediate.md#ssh-intermediate)
- Editing files - vim / nano
- Pets vs Cattle
- Automation - Ansible, Chef, Puppet etc.
- Handling upgrades - unattended upgrades, managing release upgrades etc.
- Backups - application vs container vs image



## Version Control in Industry

TODO

- Rebasing
- [Pull Requests](./open_source_dev_with_git.md#4-submitting-a-pull-request)
- Code Reviews and Checklists
- Integration with CI/CD (see below)
- Pre-commit (helps with avoiding pointless PR comments)
- Tags and Releases
- Branch discipline (keeping deployable `main` branch vs separate branches for different environments)
- Release management


## Continuous Integration and Delivery

TODO

### CI

- What is a pipeline?
- Focus on Building from local components
- Options for different providers - GitHub, GitLab, CircleCI, Travis etc.
- Agnostic to providers - focus on reusable scripting, core principles learned in this course etc.
- Generation of 'artefacts'
- Security implications
- Pre-commit (possibly in prev?)
- Standardised (possibly in prev?)
- Types of testing
- Validation stages - quality gates

### CD

- Automated deployment options (could refer to Cloud Automation)
- 'Deploy Often' vs 'Big Releases'
- 'Deploy during off-periods' vs 'Deploy while team is working'


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

TODO

- Lambda Functions / Function Apps
- Managed databases
- Managed distributions (e.g. Kubernetes)
- Container runtimes
- Blob storage
- Filesystems
- RBAC
- Networking / VPNs
- CDNs
- ML


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

TODO


### Kubernetes

TODO
