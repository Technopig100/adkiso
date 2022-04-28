=======
adkiso
=======

The adkiso project features scripts and configuration templates to build installation media (*.iso* images and
*.tar.gz* bootstrap images) as well as netboot artifacts for BIOS and UEFI based systems on the x86_64 architecture.
Currently creating the images is only supported on Arch Linux but may work on other operating systems as well.

Requirements
============

The following packages need to be installed to be able to create an image with the included scripts:

* arch-install-scripts
* awk
* dosfstools
* e2fsprogs
* erofs-utils (optional)
* findutils
* gzip
* libarchive
* libisoburn
* mtools
* openssl
* pacman
* sed
* squashfs-tools

For running the images in a virtualized test environment the following packages are required:

* edk2-ovmf
* qemu

For linting the shell scripts the following package is required:

* shellcheck

Profiles
========

Archiso comes with two profiles: **baseline** and **releng**. While both can serve as starting points for creating
custom live media, **releng** is used to create the monthly installation medium.
They can be found below `configs/baseline/ <configs/baseline/>`_  and `configs/releng/ <configs/releng/>`_
(respectively). Both profiles are defined by files to be placed into overlays (e.g. airootfs ‎→‎ the image's ``/``).

Read `README.profile.rst <docs/README.profile.rst>`_ to learn more about how to create profiles.

Create images
=============

Usually the adkiso tools are installed as a package. However, it is also possible to clone this repository and create
images without installing adkiso system-wide.

As filesystems are created and various mount actions have to be done when creating an image, **root** is required to run
the scripts.

When adkiso is installed system-wide and the modification of a profile is desired, it is necessary to copy it to a
writeable location, as ``/usr/share/adkiso`` is tracked by the package manager and only writeable by root (changes will
be lost on update).

The examples below will assume an unmodified profile in a system location (unless noted otherwise).

It is advised to consult the help output of **mkarchiso**:

.. code:: sh

   mkarchiso -h

Create images with packaged adkiso
-----------------------------------

.. code:: sh

   mkarchiso -w path/to/work_dir -o path/to/out_dir path/to/profile

Create images with local clone
------------------------------

Clone this repository and run:

.. code:: sh

   ./adkiso/mkarchiso -w path/to/work_dir -o path/to/out_dir path/to/profile

Testing
=======

The convenience script **run_archiso** is provided to boot into the medium using qemu.
It is advised to consult its help output:

.. code:: sh

   run_archiso -h

Run the following to boot the iso using BIOS:

.. code:: sh

   run_archiso -i path/to/an/arch.iso

Run the following to boot the iso using UEFI:

.. code:: sh

   run_archiso -u -i path/to/an/arch.iso

The script can of course also be executed from this repository:


.. code:: sh

   ./scripts/run_archiso.sh -i path/to/an/arch.iso

Installation
============

To install adkiso system-wide use the included ``Makefile``:

.. code:: sh

   make install

Optional features

The iso image contains a GRUB environment block holding the iso name and version. This allows to
boot the iso image from GRUB with a version specific cow directory to mitigate overlay clashes.

.. code:: sh

   loopback loop archlinux.iso
   load_env -f (loop)/arch/grubenv
   linux (loop)/arch/boot/x86_64/vmlinuz-linux ... \
       cow_directory=${NAME}/${VERSION} ...
   initrd (loop)/arch/boot/x86_64/initramfs-linux-lts.img

Contribute
==========

Development of adkiso takes place on Arch Linux' Gitlab: https://gitlab.archlinux.org/archlinux/archiso.

Please read our distribution-wide `Code of Conduct <https://wiki.archlinux.org/title/Code_of_conduct>`_ before
contributing, to understand what actions will and will not be tolerated.

Read our `contributing guide <CONTRIBUTING.rst>`_ to learn more about how to provide fixes or improvements for the code
base.

Discussion around adkiso takes place on the `arch-releng mailing list
<https://lists.archlinux.org/listinfo/arch-releng>`_ and in `#archlinux-releng
<ircs://irc.libera.chat/archlinux-releng>`_ on `Libera Chat <https://libera.chat/>`_.

All past and present authors of adkiso are listed in `AUTHORS <AUTHORS.rst>`_.

Releases
========


License
=======

Archiso is licensed under the terms of the **GPL-3.0-or-later** (see `LICENSE <LICENSE>`_).
