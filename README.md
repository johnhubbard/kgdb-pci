kgdb-pci
========

kgdb-pci is a way to view PCI device space (memory-mapped BARs) from within a
Linux kernel debugging session.

Purpose
-------

People who work on Linux device drivers for PCI and PCIe hardware often find
themselves wishing for a way to access memory-mapped PCI registers, such as
BAR0, BAR1, etc, while broken into the kernel debugger. This is particularly
useful for complex, high-speed devices such as network adapters and GPUs,
because certain types of debugging scenarios require looking at both operating
system state, and device state.

This project provides such access, by patching the Linux kernel, and the gdb
debugger. The basic approach is very easy to understand:

* Map the PCI BAR's (Base Address Registers; see the PCI specification for
details) into kernel address space.

* Limit which PCI devices and functions are actually mapped. This avoids
noise, and also avoids running out of virtual address space on 32-bit or smaller
systems.

* Augment the gdb remote communications protocol, to include commands to read
and write to PCI devices.

* Augment gdb, to support the new commands.

* Augment the Linux kernel's built-in kgdb debugger, to support the new
commands.

How to get it
-------------

There are two parts to setting up kgdb-pci: patching the Linux kernel, and
patching gdb.  Each of those projects is forked here, so you'll need to
retrieve the following:

* kgdb-pci-kernel (from <https://github.com/johnhubbard/kgdb-pci-kernel>)
* kgdb-pci-gdb    (from <https://github.com/johnhubbard/kgdb-pci-gdb>)

Detailed steps
--------------

    git clone git@github.com:johnhubbard/kgdb-pci-kernel.git
    git clone git@github.com:johnhubbard/kgdb-pci-gdb.git

    cd kgdb-pci-kernel
    git checkout -b kgdb-pci-1.0 origin/kgdb-pci-1.0
    (...configure, make, install...)
    cd ..

    cd kgdb-pci-gdb
    git checkout -b kgdb-pci-1.0 origin/kgdb-pci-1.0
    (...configure, make, install...)

It was set up that way in order to avoid complicated git arrangements, such
as git submodules. This way, each project is maintained with normal git
commands. The only downside is that one must go out and retrieve the two
above projects.

Be sure to ...
----------------

This is listed (OK, buried) in Documentation/kernel-parameters.txt, but
you'll probably want to add the following to your kernel boot arguments:

    kgdbpci=all

Coming soon
-----------------

The tentative plan is to use this kgdb-pci repository as a place to put
patches for various versions of Linux kernel and gdb, as a convenience to
those who are not working with the very latest versions of those projects.

There is some time required, in order to provide a working patch set for each
major version of the kernel and gdb.

Enjoy!
John F. Hubbard
09 Oct 2012



