---
title: Career advice to a new software engineer
tags: [Mentorship, Software Engineering, newbie, coding]
style: fill
color: warning
description: Career advice I have given or received at work.
---

## Coding Skills
As a software engineer, the most important skill that has helped me is my
ability to code. It is also something that I have had to work on consistently.
I enjoy trying new things and always fallback on my coding/debugging skills in
a brand new team.

Even when my primary role in the team was not to write a lot of code, I have
found reading/writing code to be an easy and satisfying way to understand the
details of a system. Its been a big confidence booster and understanding the
details helps me innovate from the ground up. 

Good coding skills to me is the ability to -
1. Read code that your teammates have written.
2. Write code that your team can understand.
3. Write code that runs bug free in production.
4. Ability to do the first three fast, so that you have time to try new things
   and experiment.

### Readability
This section boils down to, how do you write well? The most important
ingredient to good writing is clarity of thought. You have to be clear, why you
are doing something and what you are trying to accomplish. Often, you have
clarity on the goal, a fuzzy idea of the implementation and absolutely no clue
about the structure. On brand new projects, the structure evolves as you work
out the details and will often need restructuring when you run into some
details you did not think through up front. I have always made time to rewrite
code for better structure and readability once I have more clarity about the
details. 

Your coding style should match that of your team and the people who are going
to read your code. If you stray too far, it will make it very hard for readers
to follow you. New constructs, ideas etc should be introduced gently with
sufficient introduction so that people can understand it easily. For example,
I used to work in a team where all the engineers were new to python. They found
it very difficult to understand features like list comprehension and functional
programming in python. Getting them to understand list comprehensions first,
made it much easier to use them in my code. 

A related problem that I often ran into with readability was vocabulary. One of
my teammates once used a bunch of fancy new words that I found very hard to
understand. In particular I hated the word `serde` and asked him to change it.
I later realized that this is a popular short form for
serialization/deserialization and is used in many libraries. In a new team,
project or language its always good to do code walk-throughs with your team so
that you can explain your thought process to them. Once the basic structure and
vocabulary is in place, its much easier for people to understand and read code.

#### Context/documentation
On new projects, the initial design documents went out of date very quickly.
Without the right documentation it can be hard to onboard new engineers. When writing documentation,
I have often focused on -
1. Why did we build this software?
2. What are the goals/non-goals?
3. Data-flow.
4. Control-flow.
5. Integration with the rest of the system.
6. User interface and how customers interact with it.

In addition to these, you will need guides and documentation around - 
1. A readme file with the documentation on how to set things up and start
   contributing.
2. How to run tests, especially single test.
3. How to deploy to a non-prod environment.
4. How to run integration tests and the process for checking in a change.

This can be done informally in small projects but writing it down will pay back
dividends. Looking at open-source projects with hundreds of
contributors can give you a good understanding of how to do this well. Writing
design documents takes time and you will have to justify the impact to others
on your team. A good way to prioritize documentation is to improve it each time
a new engineer joins the team. Instead of talking through the project with the engineer,
share some written documents, take feedback and improve them. This scales well
as more engineers join. 

There are also some components of a project that are prone to bugs. I have
found it useful to pay special attention to these pieces. Adding documentation
and explanation when possible is a quick fix, but sometimes it may require
a rewrite to make things more intuitive to people. There are certain components
that will require a certain level of skill before changes can be made.
Multi-threaded code is one example, where it is easy to make mistakes.
Documenting good reading material that can help onboard new engineers can help
boost productivity.
 
### Code reviews
Get your code reviewed by everyone on the team. The more people who read your
code and give you feedback, the better you can understand your readers.
I constantly learn new things from both reviewing code and also getting my code
reviewed. Its important to not be defensive and use the review process as
a platform for interacting and learning from your peers. 
Once I got tens of comments from a new engineer on my team around small tweaks
about performance. My thumb-rule about performance is to leave everything to
the compiler and focus only on things in the critical path. After I pushed back
and asked some questions about performance, he disassembled the byte code
generated and came back saying that in most cases the compiler indeed optimized
for performance. But, in some cases like strings passed into a debug function
there is no optimization and it is indeed faster to use a lambda expression
instead of a string.

Often, you really want to ship something fast and will gravitate towards the
person who understands you best. This is especially true, when you have spent
a year or two in the same team. Switching teams forces you to interact with new
people and learn new things.

### Be concise
Editing takes a lot of time and effort, but your readers will appreciate it.
Spend as much time editing and clarifying your code as you spend writing it.

### Force multipliers
There are some things that you invest on once and then get compound interest
for the rest of your life. In this section I want to highlight a few skills and
tools that have had a compounding effect on my career. Some of these skills
were very easy to learn.

For example, when I decided that I was going to be a software developer it was
clear to me that typing would be very important. I was able to learn typing
within 3 days and can type reasonably fast after that. I find it surprising to
find people who have spent a decade or two using keyboards for multiple hours
a day, but havent figured out how to type efficiently! Any skill that you think
will save you time over the course of your lifetime and will repeat, is a skill
worth learning.

I have used the following tools multiple times in my career and they have saved
me a lot of time and effort.

1. `cut | sed | awk | head | tail | diff | comp`
2. vim.
3. xargs.
4. jq.
5. psql.
6. regular expressions.
7. Kubernetes.
