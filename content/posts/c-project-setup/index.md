---
title: How to Setup a C Project
date: 2023-04-15
toc: true
categories:
    - Computer Science
tags:
    - C
    - Programming
    - CMake
draft: true
---
## Preface

Most C projects are rarely made out of a single, easy to compile source
file. Instead, they are spread out across into multiple source and header files,
some even generated during compilation. It is common for most programming
projects to have multiple compilation targets, "debug" and "release" being the
most common. Let's not forget automated tests, generated documentation, and
dependencies. Larger projects can also take a long time to compile, so compiling
in parallel can save hours on the long run. Such big projects are often worked
on by teams where each member must have the same easy-to-use environment.

There are tools available to handle this
task. [Make](https://makefiletutorial.com/) is one such popular tool, although
it lacks important built-in features like integrated testing, dependency
management, or cross-platform compilation among others.

A more advanced solution includes CMake. I have been casually using CMake for
about two year with great results, and I will detail my setup.

## Skeleton

Here is how I structure my projects from the root:

### Directories
* `/src` contains the application/library's source code: `.c` source files and
  `.h` header files. People often place header files into a `/header` directory,
  but I never found this useful.
* `/tests` contains the test files: `.c` files each containing a `main()`
  function. They are intended for individual compilation by CMake and tested
  using the integrated **CTest** utility. More on testing [later](#testing)
* `/assets` contains your resources such as sprites, music, sound effects,
  icons, UI elements, etc. This directory is most commonly found in graphical
  application, like games.
* `/doc` contains the documentation for your project. More on that in
  [Documentation](#documentation).
* `/modules` contains the external dependencies such as libraries. More on that later.
* `.git` is automatically generated when initializing a Git repository. This
  article will not cover Git, but be aware that version control is a requirement
  for any programmer. You can learn more about Git
  [*here*](https://docs.github.com/en/get-started/using-git/about-git).

### Files
* `CMakeLists.txt`, the most important root file. This file is the configuration
  of your build system. More on that in [CMake](#cmake).
  <!-- PICK BACK FROM HERE -->
* `README.md`, an introduction to your project, formatted as
  [Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax). 
Optional. Should
  explain what your project is, how to build and use it, what's your stance on
  contributions, and include your license. You can also add badges, if you're a
  cool kid.
* `LICENSE` or `COPYING`, simple text file containing your
  license. Optional. More on that in [Choosing a License](#choosing-a-license).
* `.gitignore`, files ignored by `git`. Should include API keys, build files and
  directories, IDE files, and temporary
  files. [Here](https://github.com/github/gitignore) is a repository of
  templates for most languages (be sure to rename the file you choose to
  `.gitignore`).
* `.gitmodules`, generated automatically by `git` if you use submodules. More on
  that in [dependencies](#dependencies).

## CMake

## Documentation
I do not feel very strongly about the specifics topic. As long as you explain to
your users how to build and use your software in detail, I am happy.

### Tools
I do not feel very strongly about this. Use whatever, as long as it
documents. Here are some suggestions ordered by relevancy:

* Markup

	A repository of markup files is sufficient documentation in most cases. They
    can be parsed to generate organized and easy to read documents or shared
    around as plain text. They can also be integrated with databases like
    MongoDB or cloud services like Read the Docs or GitBooks. Here is a list of
    markup languages, ordered by preference:
	
	* [Asciidoc](https://asciidoc.org/)
	* [Markdown](https://www.markdownguide.org/)
	* [ReStructuredText](https://www.writethedocs.org/guide/writing/reStructuredText/)
	
	Fun fact, this article is written in Mardown using
    [Hugo](https://gohugo.io/).

* Comments

	If you're making a single, simple library, commenting your header file
    really well might just do the trick. Reading comments is simple, and the
    users don't have to keep the documentation around in separate files, since
    the library *is* the documentation.

* Other

	* [LaTeX](https://www.latex-project.org/), difficult to install and write,
    but produces the most professional-looking documents. Luke Smith made a
    [great
    video](https://www.youtube.com/watch?v=w8EKH_fjmXA&pp=ygUQZ3JvZmYgbHVrZSBzbWl0aA%3D%3D)
    about it.
	* [Man pages](https://www.gnu.org/software/groff/), If you're writing a
   Linux terminal application, this is the go to. Otherwise, this is a lean and
   minimalist option. Man pages are written using Groff.
   * [Doxygen](https://www.doxygen.nl/), generate documentation from code
    comments. I am adding it as a warning sign. Please, don't use it. It makes
    the code unreadable, and the documentation that's generated isn't very
    good. If you don't believe me, try to learn how to use [LLVM's C
    API](https://llvm.org/doxygen/group__LLVMC.html).

### Contents
What your documentation should include depends on what your project. Not all
elements listed below will be applicable.

* Introduction

	It's always good to write a part of your documentation with the idea that
    the reader knows nothing about your program. In the introduction, guide the
    reader through the general concepts and prerequisite knowledge, and through
    a step-by-step guide of the software's usage.

* Reference

	Once users have a general understanding of your software, they will ask
    themslves specific questions such as, "How does X work", "how to do Y", "I
    have problem Z, how to fix it". That is where a reference comes in handy,
    with specific sections for concepts, command-line invocation,
    configurations, common problems and known bugs, copyright, etc. Writing a
    good table of content, or organizing your documentation files is essential.

* Example project
		
	If you are making a library or framework, having a simple example project
	showing how to use your software is very effective.

## Testing

## Dependencies

I would advise

## Choosing a License

> **Disclaimer**: I am by no means an ethical or legal authority. You are
> entitled to your choice of license, and this is not legal advice.

Choosing a license defines how you want others to use and handle your
software. This is an immensely in-depth and complicated subject, which I will
not even attempt to cover here. Instead, I will link you to the philosophy page
of the GNU project's website for those interested:
https://www.gnu.org/philosophy/philosophy.html.

I am only going to cover open source software licenses, as I am an advocate of
free and open source software. There are
[*many*](https://opensource.org/licenses/) licenses to choose from, but I will
guide you towards 3 licenses that I favor.

1. [ISC License](https://opensource.org/license/isc-license-txt/), it basically
   says "do whatever you want, I am not responsible." This is the most
   permissive license on the list, and perfect for people who do not want to
   impose *any* restrictions on anyone with a copy of the project. Note that
   this license has [public domain
   equivalence](https://en.wikipedia.org/wiki/Public-domain-equivalent_license).

2. [MIT License](https://mit-license.org/), similar to the ISC license, but it
   enforces attribution. It is one of the most popular open source licenses out
   there.

3. [GPLv3](https://www.gnu.org/licenses/gpl-3.0.html), a
   [copyleft](https://en.wikipedia.org/wiki/Copyleft) license that enforces
   derived works to be also licensed as GPL. This is a popular restrictive
   license among hardcore freedom enjoyers. It is a complicated license with a
   lot of requirements. You can learn more about the GPL
   [here](https://www.gnu.org/licenses/quick-guide-gplv3.html).
