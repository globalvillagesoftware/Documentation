#############################
`Pylint` Enhancement Thoughts
#############################

This note contains thoughts on how I would like `Pylint` to be enhanced.

**********************
Structural Recognition
**********************
Finer control over the relationship between names and structural location of
the name within a module. For example, I would like to have a different format
for constant names within a method and different again from constant names
within a class or a module. In particular I would like to be able to classify
the program structural context as follows:

* Global |br| 
  Refers to the names of objects that are exported from a module and that
  can therefore be accessed from within other modules.
* Module level |br| 
  Objects that can be used within a module but not within nested
  classifications such as classes.
* Class level |br| 
  Objects that can be used within a class but not within nested classifications
  such as methods.
* Function level |br| 
  Objects that can be used with module level functions Including nested
  functions.
* Method level |br| 
  Objects that can be used within methods including nested methods.
* Block level |br| 
  Blocks can be nested within code at whatever level, such as within a module,
  function, class or method.

Specifically, I want to be able to define regular expressions for the names of
objects that may be encountered at any level. Other uses for fine grained
structural classifications may suggest themselves in the future.

It is possible that he implementation of this feature may require modifications
to the `astroid` module that is used by `Pylint` to access the `AST` for a
module. `Python` itself, is used to generate the `AST`.

*******************
Regular Expressions
*******************

I want to modify `Pylint` to use the `regex` third party implementation of
regular expressions in place of the normal `re` implementation supplied by
`Python`. In particular, this will allow me to use nested character sets in
regular expressions, but the other features of `regex` will undoubtedly prove
useful as well.