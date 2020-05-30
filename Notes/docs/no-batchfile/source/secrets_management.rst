##################
Secrets Management
##################

Secrets are things like passwords, access tokens, and `SSH` keys that are
confidential and should not be accessible to the general public. Management of
secrets should be available only to authorized  security personnel. In general,
these secrets are often referred to as credentials and are discussed in
:doc:`credential_management`.

Secrets management is platform dependent as its' implementation relies on low
level facilities in the operating systems and it has not been abstracted to a
high level yet. I am investigating the possibility of developing low level
platform independent facilities that will make the |gv| secrets management
package completely platform independent and will obviate the need to use
packages such as `libsecret` to support the |gv| environment.

********
Concepts
********

This package should have the following capabilities to provide a platform
independent wrapper for platform dependent services:

* Create an authentication environment.
* Retrieve or access an authentication environment.
* Change the name of an authentication environment.
* Update the contents of an authentication environment.
* Delete an authentication environment.
