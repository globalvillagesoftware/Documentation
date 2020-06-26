##########################
Organizational Information
##########################

This section is being written early as it is important to document how the
various components used by the |gv| are organized before a lot of the
documentation is written. The organization is being developed as we go and is
still highly dynamic. I expect that the content of this document will evolve as
the |gv| environment evolves. The basic approach is to give an overview of each
topic, followed by a detailed examination of the elements that form part of the
topic. Finally, we revisit the high-level topic to show how the detailed topics
contribute to the high-level topic. Also, remember we are talking primarily
about computer software development and use, by both organizations and
individuals.

**********************
Organizational Purpose
**********************

************************
Organizational Structure
************************

*******************
Kinds of Structures
*******************

Before we can decide how to organize the environment, we need to understand
what kinds of structure are expected by the tools we use and how this will
affect our environment.

Development System Structure
============================

File Storage
-------------

Almost all of the system structures used in a development system are stored on
disk in a computer. At the low level, these structures are simply sequences of
bits. What gives the bits meaning is the higher level structures that we use to
interpret the meaning of a particular sequence of bits in a specific location.
The structures that are important at the high level are files and directories.
If you are a Windows based person, you might say files and folders.

* Files |br| 
  Files are a way of collecting information that belongs together. For example,
  you might have a file of information related to a particular person or maybe,
  like this document, a collection of thoughts that belong together. This
  document is stored in a file.
* Directories |br| 
  Directories, or folders, are a collection of related files where someone has
  decided that they  belong together. Directories could have been called filing
  cabinets but to keep our analogy going the concept of a filing cabinet has a
  different name in computer based systems. In `Unix` this directory can be
  found at `/home/username`, on `Windows` it is called `\\User\\username`. The
  |gv| stores various of it's components in this directory.
* File Systems |br| 
  File systems contains sets of files and directories. Files that are not
  contained in directories are said to be found at the root of the file system.
  File systems are collections of files and directories. |gv| intends to try
  and be file system agnostic so that the |gv| environment will run on any
  file system. Directories may contain other directories, forming a hierarchy
  of directories. The principal directory of interest to a |gv| user is the
  user's home directory.

All of these components are supplied by the operating system of the computer
you are using. Most operating systems provide more than one type of file
system. These are simply different ways of organizing files and folders that
are optimized for particular usage patterns and ways of maintaining the
integrity and security of the contents of the file system.

The following sections define how each tool uses file system space and how the
tool interacts with other tools. The same interaction problems are present when
the tools are loaded concurrently in memory.

Python
^^^^^^

The key concept from `Python` that affects the way |gv| operates is virtual
environments. These allow different versions of `Python` to be used for
different projects without any possibility of the version of `Python` being
used will contaminate other projects or the version of `Python` being used by
the operating system. Each virtual environment is a collection containing a
specific version of `Python` along with the `Python` based tools that support
either it or the particular |gv| project that is using it. The |gv| makes use
of one `Python` virtual environment for each user of the |gv| on a computer. A 
virtual environment is stored in a directory tree in the user's home directory
with one sub-directory for each individual virtual environment. In the case of
multiple users on a single computer, the virtual environments will be stored
globally so that they may be shared among different users who are using a
specific virtual environment, particularly the |gv| environment.

Eclipse
^^^^^^^

Git
^^^

********************
Organizational Tools
********************

This section gives a high level overview of the tools that are used by the |gv|
and what they are good for.

Operating System
================

The operating system provides the basic software environment that allows a
computer to do useful things. Operating systems provide, among other things,
file systems and a way of invoking tools, that are often called programs.

Programming Language
====================

The |gv| uses the Python programming language for all of it's development and
production work. The programming language is a tool that allows a |gv| user to
specify what needs to be done. It is a drastic sub-set of human language,
hopefully without ambiguities, that is intended to make it easy to express what
resources a program needs and how these resources are to be used.

Integrated development environment
==================================

The integrated development environment is a tool that provides an integrated
environment for invoking the tools use for application development and
management. Traditionally, tools were independent of each other and had little
or no knowledge of other tools, nor of the overall environment within which the
tools were used. They were glued together via scripts thaat were typically
developed by each individual user. Eventually, it became clear that for many
individuals, individual tools, glued together, were not an optimal approach to
software development and use. Integrated development environments arose to
address this problem and to try and provide a better way of doing things. They
are not the perfect answer and some people prefer to work in an environment
based on independent tools that have been glued together.

Editors
-------

Compilers
---------

Deployment tools
----------------

Testing Tools
-------------

Management Tools
----------------

Documentation
-------------

Modified 3rd. Party Tools
-------------------------

Pylint
^^^^^^

Regex
^^^^^
