Release Notes for the MicroMEC Blueprint Release 3
===================================================

# Summary
# What is released 

![Architecture](../architecture/junction_architecture.svg)

## Components of the release (Akraino new)

The MicroMEC release consists of

- PXE boot image, including boot loader, Linux kernel and initramfs
- Instructions on how to prepare a Linux server as a BOOTP/iscsi/nfs host 
- openSUSE+Raspberry Pi Linux distro
- Scripts for installing the other components

##  Dependencies of the release (upstream version, patches)

This release installs as example services

- [Ruuvi tags sensor service](https://github.com/rulex/ruuvimec)
- [Tensorflow model](https://hub.docker.com/r/emacski/tensorflow-serving)

The MEC-11 code is available in [Akraino Gerrit](gerrit.akraino.org).

#    Upgrade Procedures

Update image and reboot.

#    Known Limitations, Issues and Workarounds

The supported hardware is Raspberry Pi 3B and 4, and the Ruuvi tag.


