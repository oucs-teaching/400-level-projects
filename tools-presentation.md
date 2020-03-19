---
slideOptions:
  transition: slide
---

# Tools

----

## Key principles
- Whatever your key tools are, ensure that you are using them efficiently!
- Devote a bit of time to self-review: it may be worth switching tools
  - it may be worth using multiple tools, e.g., Excel followed by R scripts
- Talk to others about tools that have made the biggest improvements

----

## Efficiency
![](https://imgs.xkcd.com/comics/is_it_worth_the_time.png)

---

# Connectivity

----

## Password-free SSH connectivity

- Set up public-key authentication
- Run an SSH agent to manage use of your keys
- Can then more effectively use `ssh` within Unix shell pipes, e.g., to transfer files

----

## SSH tunnels

- `ssh` invocation facilitates three types of tunnel:
    - `-L` means connection to a local port is passed (encrypted) through the SSH link to some port on a remote host
    - `-R` means connection to a remote port connnects to a local port
    - `-D` sets up a SOCKS proxy server

----

## Using SOCKS proxy

- My `~/.ssh/config` contains:
```
host hex
        hostname hex.otago.ac.nz
        DynamicForward localhost:1080
        user dme
```

- I have an alias `ossh` for connecting via proxy:
```
alias issh="ssh -o ProxyCommand=\
'/usr/bin/nc -x localhost:1082 %h %p'"
```

- Browser can connect through proxy (e.g., use the FoxyProxy add-on to Firefox)

---

# Version control with `git` (WLOG)

----

## Gettings started
- Learning `git`
  - https://git-scm.com
  - http://swcarpentry.github.io/git-novice/
  - https://software-carpentry.org/lessons/


----

## Use it for?
- managing and collaborating on
  - your dissertation
  - your code
- one repository? multiple?
  - no right answer
- unlikely to be much contention or sharing
  - ... but use for backup is still important!

----

## Limitations of `git`
- large data support is non-ideal
    - `git-lfs` is a solution, but does stretch the typical `git` environment
    - instead share a network location for data?
        - e.g., ITS's HCS or CS's cshome
- can't checkout subdirectories only 
    - (actually you can, but easier with `svn`)
    - Submodules can be odd to use

----

## Some `git` repository sites
- https://github.com
- https://bitbucket.org
- https://gitlab.com
- https://altitude.otago.ac.nz
    - Altitude is a [GitLab](https://about.gitlab.com) installation within the Department of Computer Science

----

## Touring CS GitLab (altitude)
- https://altitude.otago.ac.nz/oerc/scrape-tweet-dm
- https://altitude.otago.ac.nz/oerc/scrape-tweet-dm/blob/master/dm-cron-minute.sh

----

## `git` good practice / conventions
- use sensible log comments
- don't include derived files
- set up `.gitignore`
    - https://www.gitignore.io

----

## Aside: `git` badges hopefully coming soon!
- CS+IS are experimenting with micro-credentials
    - digital credential you can add to, say, your personal website
    - images contain digitally signed attestations of your achievements
- intend to offer such badges for demonstrating appropriate mastery of `git`

---

# Reproducibility

----

## Motivation
- You'll probably need to rerun your experiments more often than you expect
- You want to have a portfolio of material from your project to show for your work

----

## Automation
![](https://imgs.xkcd.com/comics/automation.png)

----

## Data processing pipelines
- Ideally use scripting to automate 
- Set metadata carefully
  - Derived artefacts (e.g. graphs) can contain the ID of the experiment that generated them
  - makes reviewing and collecting information later more straightforward

----

## `Makefiles` for data processing
- just as `make` helps build code, it can run experiments
- use intermediate files to indicate whether or not stages of the experiment need to be rerun
- develop a means to rerun parts of experiments if that's what you will need
- shell-script can work fine too 
    - ... and of course use source code management

----

## Virtualisation
- Depending on your project tasks, virtualisation may be helpful
- Allows you to create a code execution environment that's:
  - portable: you can move it between host computers
  - controllable: you can start / stop / restart
  - manageable: you can snapshot and restore states

----

## VirtualBox
- For full-machine virtualisation there's always https://www.virtualbox.org
- Full virtualisation is expensive in terms of RAM and disk space
- May need to learn about how to bridge between host environment and guest environment
  - Networks can be isolated or linked in various ways
  - Files can be shared with the host
  - Devices can be shared

----

## Vagrant
- GUI for VM often unnecessary
  - e.g., if just need to use a command shell
- Vagrant https://www.vagrantup.com remote controls virtualisation engines such as VirtualBox
- Vagrant is used in [COSC412](https://bitbucket.org/dme26/cosc412-demo/src/master/)
- `Vagrantfile`s define context for `vagrant` invocations
  - e.g., port mappings between host and guest

----

## Docker
- Docker effects user-space virtualisation: shares OS kernel
  - thus is memory efficient
  - additionally integrates efficient, layered storage management
    - i.e., only deltas stored if you build on an existing Docker image
- I have compiled an (alpha-release) lesson: https://dme26.github.io/docker-introduction/

----

## Continuous Integration
- Have an automated build process? (Ideally you should)
- Continuous Integration (CI) systems run your build process whenever source files change
    - actually CI can do all sorts of tasks, including packaging, deploying, etc.
    - A change on a [GitHub repository](https://github.com/dme26/docker-introduction/edit/gh-pages/index.md) can trigger a Travis CI pipeline (WLOG) that produces a [web page](https://dme26.github.io/docker-introduction/index.html)
    - Or an example [using code](https://bitbucket.org/dme26/pipeline-example-c/src/master/bitbucket-pipelines.yml)

---

# Efficient use of LaTeX

----

## Presumed knowledge

- Will assume you know how to use LaTeX and have a thesis template
  - e.g. https://altitude.otago.ac.nz/cs-department/otago-thesis-latex-template

----

## LaTeX advice
- Try not to fight LaTeX's formatting
- Use comments in the source code for structuring and notes to yourself
- Define your own commands, e.g., use colour to highlight:
  - discussion between you and your supervisor
  - notes to yourself, e.g., placeholder text
  - to capture semantic formatting that you might later change


----

## Example LaTeX commands

```latex
\documentclass[12pt]{article}
\usepackage[english]{babel}
\usepackage[utf8]{inputenc}
\usepackage{hyperref}
\usepackage{xcolor}
\begin{document}

% There are many ways to achieve something like this:
\newcommand{\notedme}[1]{{\color{blue}[dme: #1]}}
\newcommand{\change}[2][]{\textcolor{orange}{#2}}

Blah \notedme{a comment} blah.
Some \change[first argument is
ignored: use for context-relevant comments]{modified} text.

\end{document}
```


----

## `Makefiles` and LaTeX
- Consider creating a Makefile for your LaTeX report that is also capable of running your figure generation (e.g., graphs) or possibly your experiments, too.
  - look into tools for generating graphs such as `gnuplot` and/or R.

----

## Online LaTeX - Overleaf
- [Overleaf + LaTeX Beamer = presentation slides](https://www.overleaf.com/project/5c937b2506812a0c96ad7bb5)
- [Overleaf + LaTeX TikZ/PGF = ... anything ?](https://www.overleaf.com/project/5c937bf306812a0c96ad7be8)

---

# Cloud-based tools for computing

----

## Jupyter notebooks
- https://jupyter.org allows you to test Jupyter notebooks directly
  - e.g., work through the Python, then "IPython: beyond plain Python" page
- you can also run your own instances, which will be necessary if your code requires more unusual software libraries or hardware interaction
  - https://altitude.otago.ac.nz/cosc345/jupyter-tour1

----

## RStudio
- RStudio provides a convenient GUI for R
  - The interface works well for experimenting with graphing
- RStudio Server provides a web-based platform similar to Jupyter notebooks
- Shiny is a useful tool for interactive visualisations using R

---

# IDEs, editors and debuggers

----

## Optimise your work practices
- know your tools
- learn how to use a debugger!
    - There's usually one in your IDE, or use [gdb](https://altitude.otago.ac.nz/cosc345/gdb-tour1)!
- Consider possible use of specialised code support tools
    - e.g., for [static analysis](https://altitude.otago.ac.nz/cosc345/static-analysis-tour1)

