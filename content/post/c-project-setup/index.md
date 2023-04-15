---
Author: Moowool L. Summerwheat
title: How to Setup a C Project
date: 2023-04-15
toc: true
tags:
    - C
    - Programming
    - CMake
license: CC BY 4.0
draft: true
---

## Preface

Aside from some libraries, C projects are rarely made out of a single
easy to compile source file.

Most projects are divided into multiple source and header files, with
some generated during compilation with
[flex](https://github.com/westes/flex) or
[bison](https://www.gnu.org/software/bison/). Those projects often
have multiple compiling targets, with "debug" and "release" being the
most common, and they can also have automated tests, integration with
CI systems, generated documentation, or dependencies. Not to mention
that those dependencies, if dynamic, need to be found on the
developer's device.

Those projects can also take a long time to compile, so utilizing
parallelism and only compiling uncompiled files can save hours on the
long run. Such big projects are often worked on by teams, where each
member can have a different machine, which needs to compile the
project with identical results to the rest of the team.

Addressing these requirements is a significant challenge that all
languages must face. Most languages have ways of handling those
requirements, like Go, Rust, C#, Ruby or Swift, which include their
own build system. Languages that don't have this feature included
often have good alternatives like [npm](https://www.npmjs.com/) for
JavaScript, or [pip](https://pypi.org/project/pip/) for Python.

But when it comes to C, there is no cookie-cutter solution. You may
have heard of Make, which can do a good job for small projects
(Theicfire made a [wonderful tutorial on
make](https://makefiletutorial.com/)), but a fair bit of work still
needs to be put into make files as they are essentially fancy shell
scripts.

I have been using CMake for about a year, and my experience was
stellar. Here is how I do it.

> **Note:** CMake is only one option, and I am not claiming that CMake
> is the uncontested champion.

## Skeleton

Here are the files and folders that I often have at the root of my
projects:

### Directories
* `src`, where most of the code goes. Mostly contains `.c` and `.h`
  files. People often have a different folder for header files, but I
  never found this useful.
* `tests`, where the test files go. Mostly contains `.c` files, which
  are intended for individual compilation by CMake. Resource files or
  header files with extensive test data is welcome. More on that in [Testing](#testing)
* `res`, where your resources should go, such as images, audio,
  configuration files, or data. If you're making a graphical
  application, you probably want to use resources, such as sprites and
  music for a game, or icons for menu buttons.
* `doc`, where you write the documentation for your project. More on
  that in [Documentation](#documentation).
* `thirdparty`, where the external dependencies go. More on that
  later.
* `.git`, automatically generated when using `git`. If you have not
  learned it yet, [*commit
  yourself*](https://docs.github.com/en/get-started/using-git/about-git).

### Files
* `CMakeLists.txt`, the most important root file. This file is the
  configuration of your build system. More on that in [CMake](#cmake).
* `README.md`, an introduction to your project, formatted as
  [Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax). Optional. Should
  explain what your project is, how to build and use it, what's your
  stance on contributions, and include your license. You can also add
  badges, if you're a cool kid.
* `LICENSE` or `COPYING`, simple text file containing your
  license. Optional. More on that in [Choosing a
  License](#choosing-a-license).
* `.gitignore`, files ignored by `git`. Should include API keys, build
  files and directories, IDE files, and temporary
  files. [Here](https://github.com/github/gitignore) is a repository
  of templates for most languages (be sure to rename the file you
  choose to `.gitignore`).
* `.gitmodules`, generated automatically by `git` if you use
  submodules. More on that in [dependencies](#dependencies).

## CMake

## Documentation
I do not feel very strongly about the specifics topic. As long as you
explain to your users how to build and use your software in detail, I
am happy. 

### Tools
I do not feel very strongly about this. Use whatever, as long as it
documents. Here are some suggestions ordered by relevancy:

* Markup

	A repository of markup files is sufficient documentation in most
    cases. They can be parsed to generate organized and easy to read
    documents or shared around as plain text. They can also be
    integrated with services like databases or cloud. Here is a list of markup languages, starting from my preferred:
	
	* [Markdown](https://www.markdownguide.org/)
	* [Asciidoc](https://asciidoc.org/)
	* [ReStructuredText](https://www.writethedocs.org/guide/writing/reStructuredText/)
	
	Fun fact, this article is written in Mardown using [Hugo](https://gohugo.io/).

* Comments

	If you're making a single, simple library, commenting your header
    file really well might just do the trick. Reading comments is
    simple, and the users don't have to keep the documentation around
    in separate files, since the library *is* the documentation.

* Other

	* [LaTeX](https://www.latex-project.org/), the hardest to install
    and write, but produces the most professional-looking
    documents.Luke Smith made a [great
    video](https://www.youtube.com/watch?v=w8EKH_fjmXA&pp=ygUQZ3JvZmYgbHVrZSBzbWl0aA%3D%3D)
    about it.
	* [Man Pages](https://www.gnu.org/software/groff/), If you're
   writing a Linux terminal application, this is the go to. Otherwise,
   this is a lean and minimalist option. Man pages are written using
   Groff.
   * [Doxygen](https://www.doxygen.nl/), generate documentation from
    code comments. Please, don't use it. It makes the code unreadable,
    and the documentation that's generated isn't very good. If you
    don't believe me, try to learn how to use [LLVM's C
    API](https://llvm.org/doxygen/group__LLVMC.html).

### Contents
What your documentation should include depends on what your
project. Not all elements listed below will be applicable.

* Introduction

	It's always good to write a part of your documentation with the
    idea that the reader knows nothing about your program. In the
    introduction, guide the reader through the general concepts and
    prerequisite knowledge, and through a step-by-step guide of the
    software's usage.

* Reference

	Once users have a general understanding of your software, they
    will ask themslves specific questions such as, "How does X work",
    "how to do Y", "I have problem Z, how to fix it". That is where a
    reference comes in handy, with specific sections for concepts,
    command-line invocation, configurations, common problems and known
    bugs, copyright, etc. Writing a good table of content, or
    organizing your documentation files is essential.

* Example project
		
	If you are making a library or framework, having a simple example
	project showing how to use your software is very effective.

## Testing

## Depedencies

## Choosing a License

> **Disclaimer**: I am by no means an ethical or legal authority. You
> are entitled to your choice of license, and this is not legal
> advice.

Choosing a license defines how you want others to use and handle your
software. This is an immensely in-depth and complicated subject, which
I will not even attempt to cover here. Instead, I will link you to the
philosophy page of the GNU project's website for those interested:
https://www.gnu.org/philosophy/philosophy.html.

I am only going to cover open source software licenses, as I am an
advocate of free and open source software. There are
[*many*](https://opensource.org/licenses/) licenses to choose from,
but I will guide you towards 3 licenses that I favor.

1. [ISC License](https://opensource.org/license/isc-license-txt/), it
   basically says "do whatever you want, I am not responsible." This
   is the most permissive license on the list, and perfect for people
   who do not want to impose *any* restrictions on anyone having a
   copy of the project. Note that this license has [public domain
   equivalence](https://en.wikipedia.org/wiki/Public-domain-equivalent_license).

2. [MIT License](https://mit-license.org/), similar to the ISC
   license, but it enforces attribution. It is one of the most popular
   open source licenses out there.

3. [GPLv3](https://www.gnu.org/licenses/gpl-3.0.html), a
   [copyleft](https://en.wikipedia.org/wiki/Copyleft) license that
   enforces derived works to be also licensed as GPL. This is a
   popular restrictive license for hardcore freedom enjoyers. It is a
   huge, complicated license with a lot of requirements. You can learn
   more about the GPL
   [here](https://www.gnu.org/licenses/quick-guide-gplv3.html).
