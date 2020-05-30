***********************************
`Git` Credentials Using `Libsecret`
***********************************

*Gnome* distributes a 3rd. party credential helper for `Git` that allows the
use of the `Secret` credential store for credentials storage. It can be
installed with the following procedure:

`sudo apt-get install libsecret-1-0 libsecret-1-dev` |br| 
This gets the requirements for compiling the credential helper.
`cd /usr/share/doc/git/contrib/credential/libsecret` |br| 
This takes us to the place where the source code for the credential helper
resides.
`sudo make` |br| 
This compiles the source code to create a runnable piece of code.
`git config --global credential.helper
/usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret` |br| 
This tell `Git` where to find the credential helper to be used. This command
supports any use of `Git` by the user who runs this command.