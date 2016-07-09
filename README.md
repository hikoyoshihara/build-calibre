Build the calibre installers, including all dependencies from scratch
=======================================================================

This repository contains code to automate the process of building calibre,
including all its dependencies, from scratch, for all platforms that calibre
supports. 

In general builds proceed in two steps, first build all the dependencies, then
build the calibre installer itself.

Requirements
---------------

First you need a *bootstrapped* copy of the calibre source code. This means
either untar the official calibre source distribution tarball, or checkout
calibre from github and run `python setup.py bootstrap`.

Then set the environment variable `CALIBRE_SRC_DIR` to point to the location of
the calibre source code.

The code in this repository is intended to run on linux.

To make the linux calibre builds, it uses docker.

To make the Windows and OS X builds it uses VirtualBox VMs. Instructions on
creating the VMs are in their respective sections below.

Linux
-------

You need to have docker installed and running as the linux
builds are done in a docker container.

To build the 64bit and 32bit dependencies for calibre, run:

```
./linux 64
./linux 32
```

The output (after a very long time) will be in `build/linux[32|63]`

Now you can build calibre itself using these dependencies, to do that, run:

```
CALIBRE_SRC_DIR=/whatever ./linux 64 calibre
CALIBRE_SRC_DIR=/whatever ./linux 32 calibre
```

The output will be `build/linux[32|64]/calibre-*.txz` which are the linux
binary installers for calibre.


OS X
------

You need a VirtualBox virtual machine of OS X 10.9 (Mavericks) named
`osx-calibre-build`. To setup the VM, follow the steps

  * Turn on Remote Login under Network (SSHD)
  * Create a user account named `kovid` and enable password-less login for SSH
    for that account (setup `~/.ssh/authorized_keys`)
  * Setup ssh into the VM from the host under the name: `osx-calibre-build`
  * Setup passwordless sudo in the Virtual Machine for the user account kovid
  * Install XCode (version 6.2 is the latest that runs on Mavericks). Download
    from https://developer.apple.com/download/more/
    Note that installing only the command line tools is not sufficient as Qt
    requires the full XCode.
  * Run `sudo mkdir -p /calibre /scripts /sources /sw /patches && sudo chown kovid:staff /calibre /scripts /sources /sw /patches`

To build the dependencies for calibre, run:

```
./osx
```

The output (after a very long time) will be in `build/osx`

Now you can build calibre itself using these dependencies, to do that, run:

```
CALIBRE_SRC_DIR=/whatever ./osx calibre
```

The output will be `build/osx/calibre-*.dmg` which is the OS X
binary installer for calibre.


