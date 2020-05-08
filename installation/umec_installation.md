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


            Jump Host Requirements

            Network Requirements

            Bare Metal Node Requirements

            Execution Requirements (Bare Metal Only)

       Installation High-Level Overview

        Bare Metal Deployment Guide

            Install Bare Metal Jump Host

            Creating a Node Inventory File

            Creating the Settings Files

            Running


        Virtual Deployment Guide

            Standard Deployment Overview

            Snapshot Deployment Overview

            Special Requirements for Virtual Deployments

                Install Jump Host

                Verifying the Setup - VMs

        Upstream Deployment Guide

            Upstream Deployment Key Features

            Special Requirements for Upstream Deployments

            Scenarios and Deploy Settings for Upstream Deployments

            Including Upstream Patches with Deployment

            Running

            Interacting with Containerized Overcloud

    Verifying the Setup as defined the Akraino validation feature project plus any additional testing specific to the blue print

    Developer Guide and Troubleshooting

        Utilization of Images

        Post-deployment Configuration

        Debugging Failures

        Reporting a Bug

    Uninstall Guide

    Troubleshooting

        Error Message Guide

    Maintenance

        Blue Print Package Maintenance
            Software maintenance
            Hardware maintenance
        Blue Print Deployment Maintenance

    Frequently Asked Questions

    License

    References

    Definitions, acronyms and abbreviations

