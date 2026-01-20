---
layout: page
title: "Missing Semester: UoB Remix"
nositetitle: true
---

<div class="note">
This document has been modified from the <a
href="https://missing.csail.mit.edu/"> original site</a> developed by MIT to be more relevant to UoB's CS department.  
<br /><br />
The full history of our changes can be viewed <a href="https://github.com/afnom/missing-semester">here</a>.
<br /><br />
<em>This</em> Missing Semester is brought to you by <a href="https://cssbham.com/">CSS</a> and <a href="https://www.afnom.net">AFNOM</a>.
<br /><br />
Boxes like this will indicate notes added by us.
</div>

Classes teach you all about advanced topics within CS, from operating systems
to machine learning, but there’s one critical subject that’s rarely covered,
and is instead left to students to figure out on their own: proficiency with
their tools. We’ll teach you how to master the command-line, use a powerful
text editor, use fancy features of version control systems, and much more!

Students spend hundreds of hours using these tools over the course of their
education (and thousands over their career), so it makes sense to make the
experience as fluid and frictionless as possible. Mastering these tools not
only enables you to spend less time on figuring out how to bend your tools to
your will, but it also lets you solve problems that would previously seem
impossibly complex.

Read about the [motivation behind this class](/about/).

# Practicalities

**Please join the discord!**

* **Questions / Discussions**: Please join the [Missing Semester Discord](https://discord.gg/gwgyZQRGhV)!
* **Time/Location**:
    * **Semester 1**: Mondays 16:00-17:00 in [Law LT2](https://campusmap.bham.ac.uk/search/5d6f49201e1f64009327a634?projectId=uob)
    * **Semester 2**: Mondays 16:00-17:00 in [Murray Learning Centre (MLC): UG09 (weeks 2-4), UG07 (weeks 5-9)](https://campusmap.bham.ac.uk/search/5d6f490d1e1f64009327a041?projectId=uob)  
* **Facilitators**: MS is run by Members of AFNOM and CSS, with support from Marius Muench and Vincent Rahli.



# Schedule

<ul>
{% assign lectures = site['2025'] | sort: 'date' | where:"hide", "false" %}
{% include schedule.html hideDates=false %}
</ul>


# Related Resources

<div class="note">
Please note -- these links pertain only to the original MIT content which may have been altered.
</div>

Video recordings (of the original MIT lectures) are available [on YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).

You can find posts and discssions on:

 - [Hacker News](https://news.ycombinator.com/item?id=22226380)
 - [Lobsters](https://lobste.rs/s/ti1k98/missing_semester_your_cs_education_mit)
 - [/r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/eyagda/the_missing_semester_of_your_cs_education_mit/)
 - [/r/programming](https://www.reddit.com/r/programming/comments/eyagcd/the_missing_semester_of_your_cs_education_mit/)
 - [Twitter](https://twitter.com/jonhoo/status/1224383452591509507)
 - [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J)

# Translations

- [Chinese (Simplified)](https://missing-semester-cn.github.io/)
- [Chinese (Traditional)](https://missing-semester-zh-hant.github.io/)
- [Japanese](https://missing-semester-jp.github.io/)
- [Korean](https://missing-semester-kr.github.io/)
- [Portuguese](https://missing-semester-pt.github.io/)
- [Russian](https://missing-semester-rus.github.io/)
- [Serbian](https://netboxify.com/missing-semester/)
- [Spanish](https://missing-semester-esp.github.io/)
- [Turkish](https://missing-semester-tr.github.io/)
- [Vietnamese](https://missing-semester-vn.github.io/)
- [Arabic](https://missing-semester-ar.github.io/)
- [Italian](https://missing-semester-it.github.io/)
- [Persian](https://missing-semester-fa.github.io/)

Note: these are external links to community translations. We have not vetted them.

Have you created a translation of the course notes from this class? Submit a
[pull request](https://github.com/missing-semester/missing-semester/pulls) so
we can add it to the list!

## MIT Acknowledgements

We thank Elaine Mello, Jim Cain, and [MIT Open Learning](https://openlearning.mit.edu/) for making it possible for us to record lecture videos; Anthony Zolnik and [MIT AeroAstro](https://aeroastro.mit.edu/) for A/V equipment; and Brandi Adams and [MIT EECS](https://www.eecs.mit.edu/) for supporting this class.

---

<div class="small center">
<p><a href="https://github.com/afnom/missing-semester">Source code</a></p>
<p>Licensed under CC BY-NC-SA.</p>
<p>See <a href="{{'/license/' | relative_url}}">here</a> for contribution &amp; translation guidelines.</p>
</div>
