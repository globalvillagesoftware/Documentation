##############################
Dealing with Test Environments
##############################

********
Overview
********

A test environment provides us with the ability to run machines that
provide a secure, sandboxed environment for testing any capability that might
have dangerous interaction(s) with a software developer's computer. Several
different types of test environment can be created with various trade-offs.

Test environment based on virtual machines
==========================================

This type of environment makes use of virtual machines running on a host
computer. The virtual machine provides a sandboxed environment where an
inadvertent crash does not affect the host machine and the testing environment
can be easily and quickly reset to a known good state.

A virtual test environment has several components as follows:

* Software that allows the running of one or more virtual machines. |br| 
  |gv| uses `VirtualBox` from **Oracle** to provide this capability.
* Software that provides control of the virtual environment from the command line. |br| 
  This capability is provided by the `vboxmanage` utility of *VirtualBox*.
* Software that allows communication to a virtual machine from a host machine.
  `SSH/SCP` will be used to communicate securely between guest and host
  machines in a virtual environment and to copy files back and forth.
  Communication will be from host to guest only. Host machines are assumed to
  be the the personal computers of individual software developers and will not
  be able to function as an SSH receiver unless such capability is already
  needed in the software developer's environment.


Test environment based on dedicated Hardware
============================================

Traditional test environment
============================

*********************************
Create a virtual test environment
*********************************

Install `VirtualBox`
====================

This will be done using a Python script - `installVirtualBox.py`

The following steps are needed to create a virtual test environment:
* Install `VirtualBox` on the host system if we are creating a virtual test
environment.
* Install `VirtualBox Extensions` on the host system.
* Prepare the host system as an `SSH` client.
* Prepare a skeleton *Ubuntu Desktop* virtual machine.
* Prepare a skeleton *Ubuntu Server* virtual machine.
* Prepare a skeleton *Windows Home Edition Desktop* virtual machine.

