---
layout: lecture
title: "#12: Introduction to Agile Methodology"
date: 2026-03-02
ready: true
hide: false
---

<div class="note">
This lesson is a UoB original and has been completely written from scratch by @flaberpengu.
</div>

## Introduction

The term "agile methodology" is an umbrella term that covers various methods of software engineering that all share similar characteristics and principles. While many of the methods predate the use of the term "agile", the [Agile Manifesto](https://agilemanifesto.org/) published in 2001 by a group of software engineers is often considered the formal start of the agile movement. It stood in opposition to the previously-popular so-called "heavyweight" development methodologies, often referred to collectively as the "waterfall" model, which they viewed as too cumbersome, too micromanaged, and inflexible. They believed that lighter-weight, more flexible methods of software development enabled developers to produce better quality code and products in more productive environments.

The Agile Manifesto promoted 12 key principles, as well as four overarching goals they prioritise and value:
- Individuals and interactions over processes and tools
- Working software over comprehensive documentation
- Customer collaboration over contract negotiation
- Responding to change over following a plan

Nowadays, agile methodology is ubiquitous in software engineering and many of the ideals and principles laid out in the 2001 manifesto are perceived as obvious, but at the time this opposed the normal practice that had been standard for decades and forced a mindset change for workers at every level, from developers through to management. In fact, many notions and concepts that either arose from Agile or are Agile themselves can often be found outside of just software engineering - any collaborative project often incorporates aspects of Agile, either knowingly or unknowingly, so understanding core ideas and some of the ceremonies that developed around them is beneficial regardless of the field/career you plan to go into.

This session will focus on some of the more common frameworks that developed as an attempt to implement the principles of the Agile Manifesto, with brief demonstrations of what these may look like in practice.

## Kanban

### Overview

Kanban arose from the manufacturing industry, originating from Toyota in the 1940s and 50s, where the engineers wanted to learn from shelf-restocking practices of supermarkets and apply it to their own Just-in-time (JIT) manufacturing environment. This enabled them to produce only the stock they needed via requests from customers, in a highly efficient manner, producing minimal overhead and waste. These same principles can be applied to software engineering - dynamic task management, rapid development, and minimal time spent blocked or in work in progress (WIP).

The core mechanism that enables this style of work is a **kanban board**. Originally physical but now often digital, "cards" are placed on the board and move between various "stages", allowing for easy, clear visual understanding of what stage of progress every task is at. Each **card** has a short description of a specific task, with an assigned member of the team responsible for the given card, and often extra information such as a priority rating or an estimate of how long it will take to complete. Each task is also assigned a certain amount of "work units", an approximate measure of how long a task is expected to take. How long it takes a unit of work to travel through the production process, from beginning to end, is known as "cycle time", and minimising this is one of the ultimate goals of kanban - cycle time is a metric of efficiency in this case, so reducing it means that the team is delivering more value and output in a shorter time frame.

Kanban typically requires a product owner, who is tasked with creating and managing the kanban "backlog" - that is, all tasks not currently allocated or in progress. This assigned position enables the rest of the team to freely pick kanban cards as they see fit and work on what is best for them without having to worry about disruption. The product owner can **respond to changing situations** by re-prioritising work items in the backlog and adding tasks if new requests or requirements appear. 

Keeping kanban efficient hinges on minimising the WIP allocated tasks - for example, if a team has a stage named "awaiting review", the product owner (and by extension, entire team) may enforce a limit of three items in that stage at any one time. This forces team members to complete stages that are potentially less popular (e.g. code review) and maintains a constantly throughput of work, avoiding pileups of unfinished work which would otherwise reduce efficiency and could lead to other issues or bottlenecks. Furthermore, this idea ensures team members focus on a single task at any given time, reducing multitasking, which often slows down overall production due to repeated context switching. This continuous flow of work lends itself well to continuous integration and continuous delivery (CI/CD), a standard pipeline in DevOps work.

Due to kanban's continuous nature, it is well-suited to many forms of work. It could be used inside IT support teams, for example, with customer tickets corresponding to cards on the board. Alternatively, kanban boards can be used very loosely as a way to simply visualise tasks in a given project, even without a dedicated product owner.

### Demo

I will demonstrate the creation of a basic kanban board in [Jira](https://www.atlassian.com/software/jira) and play around with basic features.

**TODO** - insert example images

## Scrum

### Overview

The scrum framework evolved over the course of the late 1980s and early 1990s, with some of the key developers of scrum being present amongst the original signers of the Agile Manifesto. While scrum shares many concepts with kanban in that they both work towards implementing Agile principles and ethos, they differ fairly strongly in how they achieve this. Where kanban focuses on continuous delivery and minimal interruptions, scrum is ceremony-driven and work is segmented into short time blocks called "sprints". The most common sprint length tends to be two weeks, however teams are free to work with whatever scale suits their project and industry best. The reasoning behind this time blocking of work is that scrum focuses on "continuous improvement" - the idea that knowledge and improvement comes from experience and empirical evidence. What this translates to in practice is teams that learn over time and, with self-evaluation between sprints, can improve efficiency and their ways of working dynamically over the course of a project.

A scrum team is split into a few key roles: the **product owner**, who works with stakeholders to shape project requirements (and the backlog) and is ultimately responsible for the delivery of the product; the **developers** (or **team members**) - anyone who plays a role in the development of the product, not exclusively developers (e.g. can include architects, researchers, testers too); the **scrum master**, who is responsible for facilitating the scrum method itself and often coaches about scrum techniques and workflow.

### Artifacts

In order for a scrum team to keep track of objectives and an end goal, it is important for the creation of a few core "artifacts" which guide decision and planning.

The **product backlog** can be considered as the main "to do" list of the team. It is a list of features, requirements, bugfixes, and other things that the product owner maintains and updates dynamically if customer requirements or market conditions change. For businesses and products, these items are usually prioritised for maximum value and customer satisfaction. This backlog acts as direct input into the sprint backlog.

The **sprint backlog** is a list of items and "stories" that the developers choose to focus on and implement in a given sprint cycle. This selection occurs during the "sprint planning" ceremony (see the Ceremonies section). This *can* be updated during a sprint, but this is often avoided since it can cause scope creep and affect the overall sprint goal if not managed carefully and correctly.

The **sprint goal** (or sometimes the **increment**) is the overarching goal of a particular sprint. Sometimes this will be a version or feature release, or sometimes it may be an internal project milestone - it is usually dictated by both the product owner and the team. It should be reasonable and feasible to achieve during a single sprint, and progress towards it should be carefully monitored and analysed after each sprint.

### Ceremonies

In order to utilise these artifacts, and indeed to create them in some cases, scrum incorporates some key "ceremonies". These are structured meetings, usually with strict time limitations, that have a core objective to be achieved within them. This allows for minimal developer disruption whilst still enabling a successful scrum workflow and strong communication between all parties of a scrum team.

**Sprint planning** - as the name suggests, this ceremony is when the upcoming sprint is formally planned. Led by the scrum master, the team will plan the sprint goal and scope of the work to be done, and then pull in user stories from the product backlog into the new sprint backlog. These stories must be considered feasible by all involved, and sometimes user stories from the previous sprint will carry over to the new backlog where appropriate and necessary.

**Daily stand-up** - traditionally literally held stood up in a meeting room in an office but now often remote via video calling, this is a short daily meeting (usually under 15 minutes) where all members of the team align themselves on a goal, give progress updates since the last meeting, give a plan for the current day, and note any blockers. Technical and/or in-depth discussions of any issues or blockers can be had after the meeting in "break out" sessions.

**Sprint review** - a (more informal) ceremony held at the end of a sprint, usually to view a demonstration of the work achieved in the sprint or to inspect the increment (i.e. assess progress towards the sprint goal). This meeting often incorporates shareholders and the wider interest community, where relevant. The product owner will typically review the product backlog at this stage now that the sprint is completed, ready for the next sprint. 

**Sprint retrospective** - a ceremony in which the team come together to discuss how they felt the previous sprint went. These meetings often focus on a mix of positives (what went well), negatives (what went poorly, what should we do differently next time), and action ideas (how can we implement these changes and avoid these issues, how can we continue doing the positive things we found). 

**Backlog refinement** - a ceremony in which the team, usually led by the product owner in this case rather than the scrum master, work through what is in the current product backlog, remove unnecessary stories, modify and change stories where appropriate, and add stories where new/changing requirements demand. Typically this occurs before a sprint planning session, so that only useful, relevant items are considerd for the new sprint. Some teams also choose to do this mid-sprint for the current sprint backlog, but care needs to be taken to avoid accidental scope creep, and can affect sprint goals.  

### User Stories and Metrics

In order to make project requirements and overarching goals fit into a scrum board with sprint-size tasks, the product owner splits these down into "epics" and "user stories". An **epic** is a large-scale goal or objective, usually spanning many sprint blocks, and can often be fairly vague. Epics are made up of a series of related, smaller bodies of tasks called **user stories** (or sometimes just stories). These are segments of work that are designed to be completed during single sprint cycles. For example, an epic might be "Redevelop our website". This might be made up of multiple user stories, such as "Learn user feedback on existing website", "Redesign graphics to fit new theme", "Ensure iOS compatibility" - the epic cannot be completed a monolith in a single sprint cycle, but individual user stories can. These stories can be further divided into "tasks", and yet further into "subtasks" if required. Likewise, similar epics can be grouped together into "initiatives" which span e.g. financial quarters. Continuing our example, the wider initiative might be "Improve our brand marketing" - broad, achieveable in a quarter, and dividable into numerous related epics.

In order to keep the scrum workflow moving and on-track, an **estimation metric** is needed. Where kanban has work units and cycle time, scrum introduces the concept of "story points". **Story points** are an estimate of relative effort taken to complete a user story or task. Intuitively, it makes sense, but in practice assigning story points (and getting it right) can be difficult and comes from experience. Story points are not a direct measure of hours or days, but often ends up being a proxy for it. They take into account risk, complexity, and scale of work rather than specific time frames (though many teams do assign points based on expected number of days taken until completed). When planning a sprint, it is important to not load too many story points-worth of work into the sprint backlog, and likewise during a sprint individual members should not be assigned too many story points. The key to getting this right is **team communication** - product owners have objectives and sometimes developers need to be clear that they anticipate these will take longer than the product owner may like; similarly, developers may undershoot how many points to allocate into the sprint backlog and the product owner has to make clear stakeholder requirements.

Story points play into the "continuous learning" aspect of scrum perfectly - over time, estimates are likely to become more accurate, leading to better-planned sprints and more efficient work clearance in any given sprint cycle.

### Demo

I will demo the creation of a basic scrum project in Jira, showing backlogs, timelines, story point assignments, etc. I will also demonstrate the use of a Mural board for a retrospective.

**TODO** - insert example images

## Conclusion

The modern software engineering environment is powered by Agile, either explicitly through ceremonies and frameworks or sometimes implicitly through the general guiding ethos applied in subtle and unique ways. Developing an understanding and appreciation for why Agile exists is key to becoming a strong key player in future roles regardless of industry or profession - in general, Agile ideas can be applied to any scenario, and should be considered as a general mindset. Some industries implement them readily, whereas others could do with a nudge in the right direction - and hopefully you, the reader, can help be that change.

### An anecdotal application

I personally applied an Agile approach to my entire final year studies in my undergraduate degree - initially, I set out with a full scrum framework in place, even going so far as to do retrospectives. However, since this was just me doing *every role*, the process got cumbersome and I realised this did not actually really apply the lightweight Agile ethos well. I transitioned to a simpler routine of using a kanban board coupled with what were effectively solo daily/frequent standups - I would consider what I had done, what I needed to do, sketch out a plan for my upcoming work hours, and lay out clearly any blockers. I found this to work greatly, and it introduced minimal management overhead as I could just add to, modify, or remove from my kanban backlog whenever I saw fit. This hybrid style of approach is sometimes called "kanplan" or "scrumban", but really it is just my personal approach to Agile.

## Further reading

Since Agile is so prevalent in modern software engineering, a simple Google search reveals a plethora of resources.
- [The Agile Manifesto](https://agilemanifesto.org/) - essential reading for anyone interested in the Agile ethos
- [Atlassian's wiki](https://www.atlassian.com/agile) - one of the largest providers of Agile tools for organisations and businesses, had a good wiki of explanations (especially if you love corporate buzzwords)
- [scrum.org](https://www.scrum.org/) - consultancy and certification firm, but also have a large bank of resources on scrum specifically
- [Plane](https://github.com/makeplane/plane) - a FOSS alternative to Atlassian's Jira (I haven't personally used it but really should transition, highly rated)