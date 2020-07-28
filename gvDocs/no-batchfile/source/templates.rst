#########
Templates
#########

The concept of a template is used throughout |gv| software. This section
describes what templates are and how they are used by the |gv|.

******************
What Are Templates
******************

Templates are a mechanism for generating something from a simpler source.
Examples include:

* Programming language code
* Description of data structures

Reason for using templates include:

* Managing complexity
* Eliminating repetitiveness
* Reducing clerical errors

Template content
================

In most cases, templates are used to generate structured data in some form. The
|gv| combines data definitions and the code that can generate and maintain the
result of generating output from a template into a single unit, a **Python** 
`module`. |gv| use the generated output from a template as part of the
programming environment for an application or system. It prefers to package
data together  with the code that creates and maintains the data as well as the
code that lets the objects be used as components in a larger system.

Including the code that maintains the objects along with the objects themselves
is traditional object-oriented programming practice. Part of the code that
maintains an object also serves as a public interface to the object, exposing
the ways of using and manipulating the object that were foreseen by the
application developer. This encapsulation helps ensures that objects can be
self-contained and that they do not interact with other objects in an undefined
way.

Templates normally allow three major types of customization:

* Templates can include other templates.
* Templates can contain variable data that can customize the template every
  time it is used.
* Templates may allow the exclusion of parts of the template from the generated
  object. This capability is useful where not all capability defined in the
  template is relevant to a particular object.

Generating templates
====================

The |gv| makes use of the `Jinja2
<hhttps://jinja.palletsprojects.com/en/2.11.x/>` templating engine to manage
source templates and the objects that are generated.

Template Environment
--------------------

Template are primarily a tool to make the software development environment more
productive by reducing the amount of boilerplate code that must be generated.
Templates cannot be used unless a suitable working environment is provided for
them. In general, the environment will be a composite, made up of tools from
the template supplier, merged with tools supplied by the user. This environment
will need a number of components:

Context
^^^^^^^

Describe a template
^^^^^^^^^^^^^^^^^^^

A template contains two major components:

* Human readable representation |br| 

* Machine readable representation |br| 



Template targets
^^^^^^^^^^^^^^^^

Using templates results in the generation of some output. This output is the
reason for having templates in the first place.

* Human readable representation |br| 
  This is what most templating systems do today. The output normally contains
  textual information in the form of programming languages such as **Python**
  or declarative languages such as **HTML**. This output is typically used
  as a component within a larger system.

* Machine readable representation |br| 
  This is what the templating systems work with internally.

Ability to create usable templates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ability to use templates
^^^^^^^^^^^^^^^^^^^^^^^^

Template variables
^^^^^^^^^^^^^^^^^^

Templates in development
^^^^^^^^^^^^^^^^^^^^^^^^

Templates in production
^^^^^^^^^^^^^^^^^^^^^^^

*****************
Template Concepts
*****************

This section describes some concepts that are not unique to **Python** but that
apply nicely in a **Python** environment. In further sections we will show how
they apply in a |gv| environment

Modification Capabilities
=========================

This section covers the important concepts relating to generating specific
content from templates in a *Jinja2* environment. This is not exhaustive.

Blocks and template extension
-----------------------------

A block defines a segment of code that can be replaced with another set of
code. For example, it is common in **Python** to use a base class as the
foundation for a derived class. The class may have the same methods, but
the methods in the derived class may contain code that is completely different
than the method in the base class. Thus we might have the following code in a
base class in a template called `base`:

.. code-block:: default

    class someClass():
        def someMethod(self, a, b, c=None)
        {% block Block1 %}
            We do something here
        {% endblock %}

In another template called, say, `derived` we have the following code:

.. code-block:: default

    {% extends base %}
    {% block Block1 %}
        We do something else here
    {% endblock %}

The result of rendering the template `derived` will be the following:

.. code-block:: default

    class someClass():
        def someMethod(self, a, b, c=None)
            We do something else here

As you can see here we have provided a new body for the method `someMethod`.
All the boilerplate from the template `base` has been retained, but the body of
the method `someMethod` has been replaced by different code. What has happened
is that we defined a block of code delimited by `{% block name %}` and
`{% endblock %}` which was called `Block1`. Both templates had a block of the
same name and the code in the block in the first template was replaced by the
code in the block with the same name in the second template.

.. _Jinja-logic:

Logic and Variables
-------------------

Templates also support embedded logic that can change the content of generated
code. Thus the following code:

.. code-block:: default

   {% if {{ x }} == 5 %}
       Do something
   {% else %}
       Do something else
   {% endif %}

gives the following results when rendered:

.. code-block:: default

   Do something else

when the variable `x` has the value 6 and:

.. code-block:: default

   Do something

when the variable `x` has the value 5. In effect, the template renderer has
used the value of the variable `x` to determine which code to generate.

Variable values can be inserted into the template from outside when the
template is rendered. They are normally distinguished through the following
notation `{{ variable }}`. Variables can be substituted directly into text as
shown in the following example:

.. code-block:: default

   This variable has the value {{ variable }}

Assume that `variable` has the value `Some value`. The result will be:

.. code-block:: default

   This variable has the value Some value

Variables can also be used in *Jinja2* control statements such as `% if  ... }`
as shown in the example above. See :ref:`Jinja-logic`.

Macros
------

Macros are a bit like variable on steroids. Imagine being able to parameterize
a variable and you have a macro. The syntax is different but the result is
similar. The macro invocation is replaced by the text generated by evaluating
the macro. The example below shows the result of using a macro:

.. code-block:: default

   {% macro modify(a, x=0, y=None %}
    print([a, x, y])
    {% endmacro %}
    
    Some text here followed by a macro invocation: {{ modify('xyz', x=5) }}

The following code will be generated:

.. code-block: default

   Some text here followed by a macro invocation: print(['xyz', 5, None]

You can see how the macro parameters got substituted in the generated text.

Real world example
------------------

*******************************
How Does the |gv| Use Templates
*******************************
