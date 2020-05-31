#########################
Tool Enhancement Thoughts
#########################

This note contains thoughts on how I would like the development tools for the
|gv| to be enhanced and how to do this.

************
Requirements
************

`Pylint`
========

Structural Recognition
----------------------
Finer control over the relationship between names and structural location of
the name within a module. For example, I would like to have a different format
for constant names within a method and different again from constant names
within a class or a module. In particular I would like to be able to classify
the program structural context as follows:

* **Global** |br| 
  Refers to the names of objects that are exported from a module and that
  can therefore be accessed from within other modules.
* **Module level** |br| 
  Objects that can be used within a module but not within nested
  classifications such as classes.
* **Class level** |br| 
  Objects that can be used within a class but not within nested classifications
  such as methods.
* **Function level** |br| 
  Objects that can be used with module level functions Including nested
  functions.
* **Method level** |br| 
  Objects that can be used within methods including nested methods.
* **Block level** |br| 
  Blocks can be nested within code at whatever level, such as within a module,
  function, class or method.

Specifically, I want to be able to define regular expressions for the names of
objects that may be encountered at any level. Other uses for fine grained
structural classifications may suggest themselves in the future.

It is possible that the implementation of this feature may require
modifications to the `astroid` module that is used by `Pylint` to access the 
`AST` for a module. `Python` itself is used to generate the `AST`.

Use `Regex`
-----------

I want to modify `Pylint` to use the `regex` third party implementation of
regular expressions in place of the normal `re` implementation supplied by
`Python`. In particular, this will allow me to use nested character sets in
regular expressions, but the other features of `regex` will undoubtedly prove
useful as well.


`Regex`
=======

Matched string size
-------------------

It should be possible to control the size of a matched string independently
from the specification of the patterns involved. Currently, this capability
only allows the control over the consecutive number of regular expressions of
a given type that may be encountered. For complex expressions, the size of
the matched string cannot easily be associated with the regular expressions
defining a group. Attaching this capability to a group or to the entire
matched string would be helpful. The capability should be specified in the
same way that group repetitions are currently specified i.e it should be
possible to specify the minimum and/or maximum length for each group.

Support `Mypy`
--------------

A stub file should be supplied with `regex` that can be used to determine the
static variable types that are used in the implementation so that this
information can be used by `mypy` when it analyzes `Python` source code for
consistency in the use of variables. The code should also be upgraded to use
typing.

******
Design
******

The first step is to fork the tool repositories on GitHub and then clone them
to our local development machine. This ensures that we can maintain up to date
copies of the source code for all tools.

Astroid
=======

This is a low level library that supports access to the `Python` `AST` and
helps make the use of the `AST` easier. It was originally developed to support
`Pylint` but has uses with any `Python` based package that needs to access the
`AST`. It is handled first because `Pylint` depends on it.

The following work will be needed:

* Upgrade code to `PEP 8` standard |br|
  Passes default `Pylint` and `PEP 8` standards testing. Need to re-check with
  application of |gv| standards as enforced by `Pylint`.
* Upgrade code to use `Python` typing |br| 
  Riddled with error messages from `mypy`. Both code typing and a `Python` stub
  file are needed.
* Upgrade documentation to |gv| standards |br| 
  Need to define what |gv| standards are.

Release Program
---------------

* Release 0.3.0 - Code upgraded to `Pep 8` standard
* Release 0.6.0 - Code uses `Python` typing
* Release 0.8.0 - Documentation upgraded to |gv| standards |br| 
  Package enters beta testing phase.
* Release 1.0.0 - General package availability |br| 
  Completed beta testing - ready for general release.

Regex
=====

Pylint
======
