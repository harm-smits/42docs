---
layout: default
nav_order: 6
title: minishell
permalink: projects/minishell
parent: Projects
has_children: false
---

## minishell

MiniShell will introduce you to the world of shells, which provide a convenient
text interface to interact with your system. Shells might seem very easy to
understand but have very specific and defined behaviour in almost every single
case, most of which will need to be handled properly.

## Prerequisites

Your shell needs to do a bunch of stuff, so just make sure you don't do the
following stuff:
- Have very spaghetti code which makes adding stuff in the future very hard, as
doing it properly will make it easier to do projects like 21sh and 42sh;
- Wildcard and priority support will take you another month at the least, if
you want to do them, make sure you have the according amount of time to do them;
- Do not leak memory and leak file descriptors (also not in your child processes
on command execution);
- Your shell must not crash in any scenario, its a shell, the only possible
interaction you have with your OS, dont let it crash as then you shall be
doomed.

## Getting started

- There is a github repository that obeys the order of proceedings pretty nicely:
https://github.com/Swoorup/mysh
- Please make sure to do a `lexer` -> `parser` -> `expander` -> `executor` to
make your life easier. [Here](https://www.cs.purdue.edu/homes/grr/SystemsProgrammingBook/Book/Chapter5-WritingYourOwnShell.pdf)
is a solid start.
- Make sure that you understand the [shell syntax](https://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html)