# MicroMEC Installation

# Introduction

The installation on the preferred hardware (Raspberry Pis) is done simply by booting the system over network.

![Deployment](../testing/test_env.svg)

Instructions on how to network boot a Raspberry Pi are in https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/net_tutorial.md.

In other words, the kernel image, initramfs and root file system are all stored on the server. 

This means that upgrading the software can be done simply by replacing the kernel or the file system on the boot server, and rebooting.

The installation image is available at .... . We are also using the github page https://github.com/MicroMEC/documentation/ for development.

# License

Apache license

# Deployment Architecture

We are currently deploying a single node system with the installation.

## Pre-Installation Requirements

The boot server must have the TFTP/BOOTP server and NFS server installed.

# Hardware Requirements

## Minimum Hardware Requirements

Raspberry Pi 3

## Recommended Hardware Requirements

Raspberry Pi 4

# Software Prerequisites

 ## Database Prerequisites

None

## Jump Host Requirements

TFTP server, NFS server, dhcp server. 

## Network Requirements

The network must allow all messages needed for the network boot to happen. For instance, the client is not able to send VLAN-tagged traffic when it boots. Notice also that dhcp relaying is not reliable, so it is best if the client and server are connected to the same switch or have a direct connection.

# Installation High-Level Overview

## Bare Metal Deployment Guide

As above.

## Virtual Deployment Guide

Booting the image with a virtual machine is not supported. However, a virtual machine image (Vagrant file) is under construction.

           
## Verifying the Setup as defined the Akraino validation feature project plus any additional testing specific to the blue print

It should be possible to connect to the system with normal kubernetes commands. See k3s documentation.

## Uninstall Guide
 
Reboot :-)
