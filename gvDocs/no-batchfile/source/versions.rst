##########
Versioning
##########

This is another example of a seemingly simple concept with wide-spread
ramifications.

Most software is dynamic. It's content changes over time as new features are
developed and made available to users. Versioning is an attempt to give users a
very succinct description of the history of availability of a given software
package.

Originally, the primary job of versioning was to help to identify what software
was being used. Over time, it has morphed and frequently tries to provide more
information about the software that relates to using it. A version number
started simply as a sequential number that was incremented every time new
software was made available to users. Thus users could see if the particular
software copy was old or new. Over time, it's meaning and format has expanded.

Lets look at some common methods of versioning software and then we can
come back to the more general problem of identifying software.

********************************************
`Semantic versioning <https://semver.org/>`_
********************************************

Semantic versioning is quite old. It came about as one way of solving a
particular problem facing users, namely how can the user determine if new
software will run properly in the user's environment.

Two important considerations arise from semantic versioning:

* Structured numbers with semantically important components.
* The importance, to users, of communicating the potential consequences of
  using new software.

Both points show how semantic versioning is designed to help users understand
the potential impact of software on their environment.

The major problem with semantic versioning has two-fold:

* An evolution in the size and complexity of software packages.
* Changes in the meaning and use of terms.

These changes are interrelated. A couple of examples show how things have
changed in some environments.

Breaking changes
================

A breaking change is one where existing user code will stop functioning
properly because of some change in  the software that it is using. The problem
that semantic versioning has is `how extensive is the breakage?`. A breakage
that affects two places in user code has much less impact than one that affects
10,000 places. A breakage that can be upgraded with a software tool has less
impact than one that requires manual changes in the user codebase.

Some organizations have taken this approach to breaking changes. They make one
or more software releases that support both behaviors, and deprecate the
behavior that is about to break in one release while allowing users to migrate
to the new behavior at their convenience. The **Python** community has a
`package` called `futures.py`. On an ongoing basis, it contains the new
behavior for most outstanding breaking changes. When the deprecation time limit
is reached, the support for the new behavior is removed from this package and
the new behavior becomes the default. Old versions of major breaking changes
are only terminated on release boundaries.

Another problem area is patches. The advent of `continuous integration` has
greatly changed the meaning of a patch. When a software application has 1,000
or more components, each of which can change individually, The traditional
meaning of a patch becomes problematic. When one or two patches are released
daily, as in *Ubuntu*, the rationale for a patch component in a version
number becomes much less apparent. It becomes questionable to increment a patch
release level two times a day. The deployment issues concerned with patches in
a `continuous integration`scenario tend to make patch levels in version numbers
become irrelevant.

`Continuous intgration` provides another 

In general, semantic versioning became more useful to software developers than
to application users but the inherent rigidities had an impact on developers.

**************************************************
`Calendar based versioning <https://calver.org/>`_
**************************************************

The rigidities that have become apparent in semantic versioning have lead some
people to take a different approach to versioning. The original idea of a
version was to be a unique identifier that tried to make it clear what software
was being talked about. It took the form of an increasing sequence number which
was incremented by 1 at each release.

Such a number served the purpose of distinguishing between different releases
but gave no more information about the nature or content of the release. One
thing that it did do is to give an easy way of determining how many releases
have occurred. This is used, by many, as an attempted measurement of the nature
and quality of the support given by a developer to the application.

The constraints involved in semantic versioning have led some people to explore
alternatives. The most popular alternative is a scheme called `Calver` or
`Calendar Versioning` which simply says that each version will be identified by
the date on which it was issued. This date could be he year only or it could be
a combination of year and month.

********************************
Versions and Software Management
********************************

**************
Implementation
**************

It is important that any support from the |gv| for versioning be agnostic about
any semantics associated with a version number. This includes refusing to
encourage or discourage any particular semantics. This software is used by
software developers and each one has their own ideas on how a software project
should be organized and how strongly software management principles should be
reflected in the version number.


