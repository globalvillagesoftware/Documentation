#####################
Credential Management
#####################

********
Concepts
********
In general there are two big problems related to security. These include:

* Keeping data transmission secure |br| 
* Verifying that clients are authorized to use the service. |br| 

Managing credentials for authentication is a platform dependent exercise. Each
platform provides its' own type of service that allows one to store credentials
and retrieve them when necessary. Each service stores the credentials in an
encrypted form that means that an intruder, even if they can find the physical
place where the credentials are stored, cannot read them or make any use of
them.

The way that this service is offered is very platform dependent as are the
details of the capabilities offered by the service. This means that, if we want
to be platform independent, we must implement a platform independent interface
to platform dependent modules.

The functions needed, conceptually, are quite simple. We need to do the
following:

* Create credentials that are stored in the credential management system.
* Retrive the credentials when they must be presented to organizations that want
  to validate access to their services.
* Update the credential information as the actual credential information or
  information about the use of these credentials changes,
* Delete the credentials as they expire.

This process is known in the software industry as CRUD and it occurs in many
situations. We need to Create the credentials, Retrieve them, Update them, and
Delete them. Thus our platform independent system will offer these capabilities
and will interface with platform dependent components that will access
the actual operating system capabilities that implement these requirements.

************
Requirements
************
One of the biggest problems in dealing with authentication protocols is the
variety of authentication methods and associated data used. In |gv|
the following methods are most significant:

* Password authentication |br| 
  This is used to allow an entity to login securely to a site that needs to
  protect access to the information maintained by that site. This type of
  protection requires that the website stores a copy of the username and the
  password of the user and uses this to authenticate the identity of the client
  and thus ensure that access to the website is allowed. This mechanism does not
  address the problem of data integrity.
* Token authentication |br| 
  A token is a single document that encapsulates both the identity of an entity
  along with data that verifies that the entity is not counterfit. Tokens
  are used between entities and a single organization that issues them and are
  not normally useful for authentication to other organizations. This mechanism
  is not useful for preserving data integrity.

* Symmetric keys |br|
  These keys are used to provide encrypted communication between two sites,
  normally on the Internet. The same key is used to encrypt the traffic and to
  decrypt it. This means that both the sender and the receiver need to have the
  same key if they want to communicate. Symmetric keys support data integrity
  only by encrypting traffic that flows between two entities and does nothing to
  support the authentication of clientsusing the keys. The following  problems
  exist with symmetric keys:

  * Safe exchange of keys prior to use |br| 
    Since symmetric keys are of little use if the receiver of a transmission
    does not have a copy of the key so that transmissions can be decrypted, a
    method has to be found of securely getting the key to the recipient before
    its' use.
  * Safe management of a compromised key |br| 
    if a symmetric key is lost or compromised in any way, it must be revoked. If
    a key is compromised at a recipients' site, the issurer of the key may not
    know what has happened and can continue to use the compromised key with
    other recipients, negating any security that should have been provided by
    the symmetric keys.
  * Overhead in dealing with many users |br| 
    Since secure material has been given to many users, it beomes a problem to
    maintain security over all these copies.

* Asymmetric keys |br|
  These keys were introduced as a secure replacement for symmetric keys. An
  asymmetric key is actually a pair of keys. One key is used to encrypt data
  while the other key is used to decrypt the traffic. if the data is intercepted
  during transmission it can only be decrypted using the key that has been kept
  private. Not surprisingly, these keys are called public and private keys
  respectively. The public key is distributable, publically, to whoever needs it
  and the private key is kept private in the hands of the relevant organization
  or website that made the public key available. If a public key becomes lost,
  no harm has occurred. The loser will not be able to communicate with the owner
  of the key pair until a new copy of the public key is obtained.
* Certificate authentication |br|
  Web-sites have a special problem. A single web-site may have clients running
  into the millions. Distributing and maintaining public keys manually can
  become a formidable task. Certificates remove this problem and obviate the
  need for the web-site client to store any keys to be able to access the
  web-site. If the  web-site needs to control who can access the web-site, they
  typically also require the client to login with a user name and password or
  with an access token in addition to using the certificate to provide data
  integrity. |br| 
  Certificates work by sending a public key, along with a symmetric key that is
  used for one session only and that provides the actual data integrity services.
  The public key is used for transmission during the handshaking protocol that
  establishes a connection. Once the session has been established, the
  symmetric key takes over to provide data integrity services.

The immediate need is for support for password authentication as that is the
commonly used authentication mechanism and will be required to support the
authentication needed when many of the software development functions are
automated.

******
Design
******

Initially, this support will be implemented for a *Linux* platform, but since
the |gv| is supposed to be platform neutral, it will also be implemented
for *Windows* as soon as possible to support the continued platform independence
of the |gv|.

**************
Implementation
**************

The first step in implementation is to install the necessary prerequisites for
the implementation of this capability. These include the following:

*Python* interface to `libsecret`, available from *Gnome*. It is turning
into a nightmare to find where this interface resides, what is its' name on
*Ubuntu* and how to install it. It turns out that we must use `pip install
PyGObject` to get the *Python* interfaces to the *Gnome* components.