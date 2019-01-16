---
# Featured tags need to have the `list` layout.
layout: list

# The title of the tag's page.
title: Projects

# The name of the tag, used in a post's front matter (e.g. tags: [<slug>]).
slug: projects

# (Optional) Write a short (~150 characters) description of this featured tag.
description: >
  ![image colorful math books](/assets/img/header-img/sharp_colorful_mathbooks.jpg)
  Summaries and links to projects I am currently working on or have recently
  completed
# Setting `menu` will generate an entry in the sidebar for this tag.
menu: true
order: 4
---
At any given time I have several ideas, some quite ridiculous, but others end up
making it into reality. Assembled here are some original ideas and many more
functional tools modified from the very clever work of others. I will be adding links to the repos as they become useful and I feel like bringing them out in public! 

---
### Encalculable

`Encalculable` is an open-source package for helping coders, scientists, and
engineers with dyscalculia or dyslexia. It does this by color-coding the digits
0-9 or letters A-Z such that commonly transposed letters and/or numbers are
colored with complementary colors from the color-wheel (_e.g.,_ yellow
and blue, magenta and green, etc. _see figure_)

<a title="DanPMK at English Wikipedia [GFDL (http://www.gnu.org/copyleft/fdl.htm
l) or CC BY-SA 3.0
 (https://creativecommons.org/licenses/by-sa/3.0)], from Wikimedia Commons"
href="https://commons.wikimedia.org/wiki/File:RBG_color_wheel.svg"><img
width="512" alt="RBG color wheel" src="https://upload.wikimedia.org/wikipedia/
commons/thumb/a/ab/RBG_color_wheel.svg/512px-RBG_color_wheel.svg.png" style=
"float:right;width:150px;height:125px"></a>

The user will be able to install the package, choose a good color scheme for
their use, and easily tweak a number of syntax highlighting settings in a Python-
script command line interface tool that generates the appropriate changes in the
global Vim or Neovim configuration files. The user may also be able to select
color-deficiency-friendly color schemes and the package will adapt the
highlighting appropriately.

If it works, the end objective is to assemble packages for various text applications and web browsers instead of just Vim or Neovim plugins. However, I need to work out the specifics of how to arrange the color pallette at the moment. 

<!---[Encalculable on GitHub!](https://github.com/jjradler/encalculable) [Encalculable on GitHub!]\-->

---
### Python Time-Frequency Analysis Visualization Toolkit
In my role as a researcher, I have needed to perform time-frequency analysis on
large time series. In some cases I have required visualizations that are not
part of the standard toolkit in most plotting software, so I cobbled together a
program that parses the time series output from your simulation software or data
 collection rig and creates a 2-dimensional array of time-resolved frequency
spectra (*i.e.,* as the window slides along the time-domain, a new frequency is
generated and the evolution of features in the frequency domain may be tracked
with respect to time.)

I will explain the methodology and construction of the package in a later
blog post.
#### Coming Someday...
Eventually I'm going to refactor this into something a little faster, like Julia
or C++, but for the time being, this has been quite useful for me as a
computational chemist studying dynamical molecular processes.

<!---[Time-Frequency Analysis (TFA) Visualization Toolkit](https://github.com/jjradler/tfa_tools --> 

---
### Dotfiles

My own special blend of dotfiles for setup on a personal or work machine. Some
of my colleagues have called it:

* "Inspired!"
* "Colorful!"
* "Ugh, an Eyesore!" (thanks, DBWY... appreciate it...)

It's set up so there's high contrast and easy readability and differentiation of
various filetypes, directories, etc. There is also an obscene number of color-
schemes for Vim/Neovim in this repo.

#### Coming soon
Automated global system configuration and tool installation.
Admittedly, this one is a long way off at the moment. I have a lot of tools, and
writing a repo so it's *easily reconfigured* and generalizable is quite a
challenge sometimes. Stay tuned though!

<!--[J's Special Blend Dotfiles on GitHub!](http://github.com/jjradler/special-blend)-->

---
### <!--PopTent Project Builder-->
<!--Quick and dirty little shellscript that automates the setup of filesystems for-->
<!--projects.-->

<!--Why on Earth would you need such a device?-->

<!--*Well*, if you're anything like me, you're a little bit preoccupied with, oh, I-->
<!--don't know --- **investigating the very heart of the Known Universe** --- so you-->
<!--tend to let your filesystems decay into absolute anarchy during the initial-->
<!--flurry to get ideas up and running.-->

<!--That's where the PopTent Project Builder comes in. It will generate the necessary-->
<!--license and README files and directory structure (after a little configuring,-->
<!--of course) so you can focus on filling those pristine folders up with all of-->
<!--your borderline-insane ideas, data, and code snippets.-->

<!--Right now it's cusomized for my tastes as a chemist studying quantum-->
<!--mechanics. When I get a chance I will generalize it a bit and make it more-->
<!--usable for a wider audience.-->

<!--[PopTent Project Builder on GitHub!](http://github.com/jjradler/pop-tent)-->
#### <!--Also coming soon!-->
<!--Setup of a TeX folder and the appropriate .gitignore to-->
<!--automate the initialization of your publications and documentation. Is there a-->
<!--market for this?  I have _no idea_, but I will personally find it useful!-->

---
### Gaussian 16 Input-Output Syntax Markup for Vim/Neovim
A set of `*.vim` files for syntax highlighting of keywords and various other
features of interest in Gaussian input (`*.com`, `*.gjf`) and output
(`*.fchk`, `*.log`) files. These were not built from scratch, but are based on
various other iterations cobbled together and heavily modified to suit my
tastes as a researcher in quantum dynamics using the various development and
deployment versions of Gaussian.

Admittedly, this is just my added modifications to a delightful set of these
files I found on GitHub.

<!--[J's Fork of this Delightful Repo](http://github.com/jjradler/gaussian_syntax)-->

---
### Parsers and Automation of Various Types for Gaussian Logfiles
There comes a time in everyone's life when they are just too cool to deal with
slogging through incredibly long, verbose logfiles looking for a specific keyword or value.

And by that, I mean they're too tired and under the gun to do the job well.

I was struggling with reading thousands of lines of logfiles from Gaussian in my
role as a computational chemist. I was making so many mistakes that it was really
irritating my research advisor (thanks, Boss!), so I wrote some parsers to do that
particular job for me. The collection is written in Bash and Python, but I think in the near future I’d like to assemble it all into a Python package that could be deployed generally for various types of computational software logfiles. I just happen to be more intimately familiar with the Gaussian computational chemistry software logfiles than any others at the moment. 

Other parts of this kit include:
* Automation for writing batches of Gaussian input files
* Automation for writing batch scripts on the UW Mox Cluster ([thanks, UW-HPC!](https://itconnect.uw.edu/research/hpc/))
* Matrix parsers
* Selective frequency normal mode parsing
* Excited state spectra parsing from dynamics simulations in G16
* Time-evolving observables in Mixed Quantum-Classical Dynamical simulations
* Geometric distance parsing between particles, output as a handy time-series

<!--[Automating the boring stuff in Computational Research on GitHub!](http://github.com/jjradler/gaussian_automation)-->

---
### <!--I've also got this crazy notion...-->

<!--To start refactoring some open-source numerical packages that Julia needs in its-->
<!--standard library. I don't know which ones to start with --- any suggestions?-->

