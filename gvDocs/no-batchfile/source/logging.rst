
.. _logging-section:

*******
Logging
*******

The basic principle is to do as much as possible using the **Python** logging
capabilities as possible. The basic logging package is very flexible and it
appears to be capable of doing most of the things that we want.


The standard logging system, supplied by Python is very flexible but does not
handle certain cases well. In particular, logging systems, in general, need  to
offer greater support when matching their capabilities against the needs of an
organization in a geographically dispersed environment. To understand why this
is so, lets review the functionality that needs to be supplied by a logging
system.

Logging system principles
=========================

There are several basic matters to be considered when logging:

Why should a message be logged?
-------------------------------

Some reasons why a message might be logged include:

* A catastrophic error has occurred in the operating system environment.
* A system error has occurred.
* An application oriented system error has occurred.
* An error in using the system or application has occurred.
* Stakeholders want information about the normal operation of an application.
* Developers need internal information to troublehoot problem.

These reasons align well with the logging levels supplied by the **Python**
logging system. Thus:

* `Critical` is a message about a catastrophic error in the environment. |br| 
  Examples might include a power failure that shuts down a data-center or the
  failure of a critical piece of hardware such as a database server.
* `Error` is a sytem or application error such as a crash of an application or
  the detection of an abnormal state during application operation.
* `Warning` corresponds to messages that documnt what appears to be a user
  error.
* `Info` corresponds to information that might interest a stakeholder.
* `Debug` correspond to information that is not normally needed but which can
  help developers or maintenance personnel diagnose and fix a problem.

What does the message look like?
--------------------------------

There are several parts to a logging message:

Message content
^^^^^^^^^^^^^^^

This is the core of a logging message and is always represented as text. Any
customization of the logging message text should be done in the application
before the message is submitted to the logging system.

Application specific contextual information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is possible to associate application specific contextual information with
the logging message, either when the message is submitted to the logging
system, or later in a customized logging component if this approach is more
practical. Examples include such things as the name of the module issuing the
message or the information that defines a communication channel.

Logging system specific information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

System specific contextual information is essentially the same as application
specific contextual data. The only difference is that the information is about
the system environment that was present when the logging message was generated.
Examples could include the time-stamp associated with the logging event or the
name of the computer involved with the logging event.

Where should the message go?
----------------------------

This depends upon the nature of the problem and the types of people that an
organization considers useful for working with the class of problem represented
by the message. Current logging systems support a message being delivered to
several destinations which helps the requirement that logging messages should
arrive at places where they can be acted on. A message that is poorly
classified runs a high risk of ending up in limbo and never being acted on.
Verifying that messages arrive where they should is part of application
integration testing, and this activity should not be neglected.

Logging system configuration
----------------------------

The logging system can be configured to write all logging messages to stderr
and this will be the default configuration if nothing more complex is
supplied.
The configuration of the logging system will be read from a configuration file
and will be modifiable through the system configuration data and setup in the
logging initializer. We make use of the Python logging system by default but it
is intended to allow this to be overridden by the user if a different logging
system is desired.

**Python** logging system design
================================

Overview
--------

This information is presented to help potential users understand how the
**Python** logging system works and how it can be exploited by application
logging systems.

The **Python** logging system is made up of a number of components that work
together:

* Loggers
* Filters
* Formatters
* Handlers
* Log Records

These components all work together to make the logging system work. They are
all customizable by applications, when needed, to support the exact type of
logging environment needed by an application. In addition the environment
within which logging takes place needs to be considered as part of the design
of logging for a particular environment.

Logger
------

This is the controlling component of a logging system. All requests for logging
pass through this component and it is ultimately responsible for causing
logging to occur.

`Loggers` can be composed into one or more hierarchies. The normal practice is
for `loggers` to propagate messages to `loggers` that are
higher in a hierarchy. This means that several `loggers` may get to look at a
specific message and to decide what to do with it. If propagation is turned off
for a specific `logger` the message goes no further in the hierarchy than the
`logger` that has propagation disabled.

`Loggers` have names such as `a.b.c`. Here `logger` `a` is at the root of the
hierarchy, `logger` `a.b` is under `logger` `a` and `logger` `a.b.c` is the
`logger` at the opposite end of the hierarchy to `logger` `a`. It is possible,
in this scheme, to have `logger` `a.b.d` which is at the same level as `logger`
`a.b.c` but is independent from it. The only significance of the name of each
`logger` is that it uniquely identifies a `logger` in the hierarchy. Note that
`loggers` `a.b` and  `c.b` are different `loggers`. The fact that `b` is the
leaf node in both hierarchies is irrelevant. This naming style should be
avoided since it can give rise to confusion for human users.

`Loggers` do not actually log messages unless they have a `handler` associated
with them. They may simply reject messages, based on the decision of the
attached `filters`, or they may alter the appearance of a message through
associated `formatters`. They can also decide whether a message should be
passed further up the logging hierarchy.

Filter
------

`Filters` are the engine that makes **Python** logging such a powerful tool.
Their primary purpose is to accept or reject logging requests based on
arbitrary data and rules that are associated with each logging message.
`Filters` are more powerful than just described as they see every message
presented to a `logger` and have the power of making arbitrary changes to the
message and it's associated data.

Any number of `filters` can be attached to a `logger` or to a `handler`. This
permits each `filter` to handle  one condition, if desired, or testing for
multiple conditions can be handled by a single `filter`. The application
requirements and potentially, performance requirements if the logging volume is
high, will drive the choice of implementation.


Formatter
---------

The job of a `formatter` is to prepare the message for final display. This is
normally done by adding information to the message. Loggers and handlers each
support a single `formatter`. It is normally good practice to refrain from
adding information to the message presentation before the last moment. The idea
is to avoid the processing  associated with formatting if the message is not
going to be logged. Formatters can only be attached to handlers.

Handler
-------

This is where the message gets logged. The `handler` is responsible for getting
the message to a physical destination. More than one `handler` may be
associated with a `logger`, so a given message may be sent to several different
destinations. When you consider that a given message may be handled by multiple
`loggers`, there is tremendous flexibility in routing a message to multiple
destinations.  Many destinations such a the console, a file or the platform's
logging systems are destinations that are supplied by the logging system, but
it is possible to write custom handlers that can send the logging information
anywhere that you want. For example, you can store it in a database which can
give utility to the meaning of all the data associated with a message. Thus
you can easyily have total control over the logging environment within an
organization.

Log Record
----------

The `Log record` is used internally within the logging system by all
components. It contains the actual message and all the data associated with the
message that has become associated with it by means of the logging system. It
is possible to implement a custom `Log Record` if different data storage or
behavioral capabilities are needed. It is unlikely that a custom `Log Record`
will be needed unless `Handlers`, `Filters`, or `Formatters`, that can take
advantage of the custom `Log Record` are also implemented.

Logging Environment
-------------------

Here we discuss the impact of the environment that contains the logging for an
application. Both **Python** specific and general concerns are addressed. If
you pay attention to these concerns, your overall logging environment will tend
to serve your organization's needs in a more optimal manner.

Python Environment
^^^^^^^^^^^^^^^^^^

Logging is very sensitive to the runtime environment supported by **Python**.
Each **Python** interpreter creates it's own logging environment and that
environment is unique to each individual interpreter. Thus it could be
possible for three or four logging environments to be active at the same time
on the same computer. The logging environment only lasts as long as the
interpreter is active.

The problem now becomes to properly consolidate the logging output from each
environment and make it available where it is needed. This is part of the job
that is managed by `handlers`. They are responsible for writing logging output 
to shared locations where the data is consolidated for access from other parts
of the organization. Examples of consolidation include writing logging data to
files or to databases.

Geographical Environment
^^^^^^^^^^^^^^^^^^^^^^^^

In geographically dispersed organizations, the location whre a problem is first
reported  almost certainly is different from the location where resources exist
that can deal with the problem. Part of a logging system is to make sure that
information is available wherever it is needed. This means that the logging
system must be capable of sending information to anywhere in the world,
potentially.

The information contained in a `Log Record` is not directly suitable for
transmission over a network. It may have to be `pickled` or converted to an
industry standard format such as `JSON` before it can be sent, and the
recipient must know what to do with such a message when it arrives.

This all means that establishing and maintaining a support environment for an
application has more ramifications and needs more interacting pieces than
appear when only looking at the surface of an application.

Organization Environment
^^^^^^^^^^^^^^^^^^^^^^^^

Application logging system design
=================================



|gv| logging system design
==========================
The full scope of logging as desired by the |gv| will be developed
incrementally in a number of phases.

Phase 1
-------

Produce the basic logging capabilities that will allow other |gv| components to
be developed.

* The |gv| family of loggers assumes the standard **Python** hierarchical, dot
  separated naming structure for loggers.
* Logging configuration data for each |gv| based logger will be found in a
  platform sensitive conventional location. For example, on *Linux* this will
  be '/etc/GlobalVillage/logging' where GlobalVillage can be changed to the
  name of the organization that generates a particular logging tree. The
  organization name will be assumed to be the same as the name of the root
  logger for an organization.
* Within a `logging` directory, directly subordinate files will be assumed to
  be global files that can be applied to any logger and essentially define
  the environmental environment for a logger which should be common across all
  loggers.
* Logging to stderr only will be supported. No application or system data will
  be supported. Only mesage will be logged.
* No filtering or custom formatting will be supported.
* The logging configuration dictionary will be used to configure logging where
  the configuration will be generated by hand.
* The logging configuration will be stored in a JSON file on disk.
* We will use the standard `logging.getLogger()` call but it will be
  invoked in a cover function provided by the |gv| that will allow us to
  use logging dictionary configuration. Except for initial configuration of the
  logging configuration, everything will be done by the standard
  `logging.getlogger()` so compatibility with existing uses of the standard
  **Python** logging environment will be preserved.
* Although streaming to stderr will be the only supported logger handler, the
  use of file based handlers such as `Rotating File Handler` will be tested.
* Fine tuning of the logging evironment and logging operation will be supported
  via configuration data and the command line when an application is invoked.
* Logging will be setup in three pahses during the initialization process.

  * A minimal environment with disk based configuration only will be
    established to allow logging to take place during application
    initialization.
  * When configuration and command line overrides have been read by the
    application initialization they will be applied to the logging
    configuration.
  * Application specific code will be able to add application specific logging
    configuration if desired.

* Shutdown of the logging system will be handled by the underlying **Python**
  logging system.

Phase 2
-------

Add the logging of application logs to a database using *MongoDB*.

* Install **Docker** on *Linux*.
* Setup a **Docker** container for *MongoDB*.
* Setup *MongoDB* to receive and store `JSON` based copies of `Log Records`.
* Use a socket driver to communicate with the database from the logging
  handler.
* The database handler will function in parallel with the `stderr` handler.
* The database will be capable of storing any application and system specific
  data that is provided to it.
* The application and system data can be used as selectors to help define the
  set of `Log Records` that can be retrieved from the database.
* Database retrieval will be done using the standard tools that come with
  *MongoDB*.

Phase 3
-------

Add the ability to optionally send |gv| relevant logging data to the |gv|.

* A separate logging environment will be used for |gv| specific logging.
* The organization can control which |gv| logging data is also retained in the
  organization specific logs. This will be done through specialized `filters`
  and `handlers`.
* This support will be designed so that it can be used by any 3rd. party
  software development organization that has software used in a foreign
  environment (the normal for software development organizations). This will
  make it easy for any software development organization to integrate cleanly
  with users of their software. It should be possible for the software from
  many development organizations to be used together and have it all cooperate.
* A special `HTTPS` based `handler` will be implemented to handle the
  communication between a user's environment and the software developer's
  environment. This will ensure that data transmissions will not be corrupted
  and that they cannot be intercepted in a useful fashion.
* Security and privcy will be of primary concern with this software.
  
  * The software user will be the primary judge of how much communication
    between different organizations and the user will be acceptable and what
    sorts of information can be transferred. In general, these mechanisms will
    be designed to promote trust between organizations and to reduce the
    possibility of unacceptable 3rd. party surveillance. Any such system can be
    penetrated by a sufficiently focused attack but it can be ensured that the
    effort required will not be trivial.
  
  * It will be possible for a user to make use of 3rd. party software to
    re-direct the output of any 3rd party logger to it's own logging facility.
    The grounds for the existence of such a facility are that any events
    generated in a user environment should be known to the user and the
    knowledge of the existence of such events should be controlled by the user.

Wrapup
======

**Global Village** logging implementation.
==========================================

Logging configuration
---------------------

.. automodule:: lib.gvLoggingConfig
   :members:

Using logging
-------------

.. automodule:: lib.gvLogging
   :members:
