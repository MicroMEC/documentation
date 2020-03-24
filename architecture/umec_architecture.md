# MicroMEC Architecture

Introduction
===============================================================================

This document presents the software architecture for the µMEC, rationales for the choices and links to references.


Requirements
=============

The µMEC software stack has several components and for each there are different alternatives. 

Here are some of the concerns for the µMEC that the µMEC software stack must address:

Linux operating system distribution with support for containers
----------------------------------------------------------------

The µMEC hardware must have an operating system with support for containers. This is because the third-party applications that the µMEC supports are deployed as containers, which have the benefit of being independent of the operating system distribution. The operating system distribution is needed for hardware support and for providing all necessary packages, some of which may not be known at the time of this writing.


µMEC lifecycle management
-------------------------

The µMEC must commissioned when it starts up. This should happen automatically so that when it starts, it will find its configuration and install the necessary software. It also must be kept up to date with the latest firmware and operating system components.

Tools such as Ansible or Salt can be used for lifecycle management.


Application management
----------------------

The MEC applications must be copied to the µMECs according to some rules. The ETSI MEC is working on this.


Application configuration 
--------------------------

The applications must be launched at the µMEC with suitable permissions from the operating system. A big part of the application configuration is the networking configuration, so that the application components will receive IP addresses and will be registered to a local DNS server. The application configuration can also include MTTQ configuration if they use it.


Application interfaces
-----------------------

The applications will both use interfaces and provide them. For this, they use the API Framework. The APIs available include the services that the platform provides and the sensors available. Applications can also use services provided by other services if that is allowed.


User management, authentication, authorization
----------------------------------------------

The µMEC will have different levels of users with varying permissions: some users will be allowed to update the software platform and some will manage their applications. It must be possible to authenticate the users and authorize them to do maintenance operations. The most likely mechanism for this is configuring ssh keys for authentication, with no password access.


Data management
----------------

The data the the µMEC applications generate must be moved out of the box to be useful. The µMEC must support the different mechanisms to connect to outside systems.


Operating System 
=================

The operating system includes, besides the Linux system to get the hardware to do something, also mechanisms to keep the system up-to-date with the latest security patches. Related to this are also the boot system and the boot trust system.


Boot
-----

The video by Alexander Graf, SUSE on Marrying U-Boot, uEFI and grub2 (https://youtu.be/qJAkJ3nmWgM) is a good introduction to the topic. The default way to boot ARM-based systems is using U-Boot. The U-Boot supports uEFI that in turn provides a standard execution enironment for operating system bootloaders. 

### uEFI ###

UEFI provides a standardized interface to board firmware. It is a standard with different implementations. It allows eg filesystem support in operating system, and from the Linux distro point of view it allows a uniform image.

The reference implementation for uEFI is Tianocore. It has good support for different Intel boards, being an Intel project, but the support for ARM processors is quite limited. (https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Platforms). 


### U-Boot ###

Das U-Boot had the first releases already in 2002 and seems to be still actively maintained, with the 2019.01 release in the works. 

The U-Boot can be chainloaded with other bootloaders. 

https://www.denx.de/wiki/U-Boot


### ARM Trusted Firmware (ATF) ###
http://infocenter.arm.com/help/topic/com.arm.doc.den0021d/DEN0021D_Trusted_Base_System_Architecture_Client.pdf



Virtual infrastructure layer
=============================

The key components in the virtual infrastructure layer are

OCI
:  Established in June 2015 by Docker and other leaders in the container industry, the OCI currently contains two specifications: the Runtime Specification (runtime-spec) and the Image Specification (image-spec). The Runtime Specification outlines how to run a “filesystem bundle” that is unpacked on disk. At a high-level an OCI implementation would download an OCI Image then unpack that image into an OCI Runtime filesystem bundle. At this point the OCI Runtime Bundle would be run by an OCI Runtime.

containerd
:   containerd is an industry-standard core container runtime with an emphasis on simplicity, robustness and portability. It is available as a daemon for Linux and Windows, which can manage the complete container lifecycle of its host system: image transfer and storage, container execution and supervision, low-level storage and network attachments, etc..  containerd includes a daemon exposing gRPC API over a local UNIX socket. The API is a low-level one designed for higher layers to wrap and extend. It also includes a barebone CLI (ctr) designed specifically for development and debugging purpose. It uses runC to run containers according to the OCI specification.

runc
: runc is a component of containerd, the executor for containers. containerd has a wider scope than just executing containers: downloading container images, managing storage and network interfaces, calling runc with the right parameters to run container 

CRI-O
:  CRI-O [#cri-o] is an implementation of the Kubernetes CRI (Container Runtime Interface) to enable using OCI (Open Container Initiative) compatible runtimes. It is a lightweight alternative to using Docker as the runtime for kubernetes. It allows Kubernetes to use any OCI-compliant runtime as the container runtime for running pods. Today it supports runc and Clear Containers as the container runtimes but any OCI-conformant runtime can be plugged in principle.

   CRI-O supports OCI container images and can pull from any container registry. It is a lightweight alternative to using Docker, Moby or rkt as the runtime for Kubernetes.

Podman
: Podman (https://podman.io) is a replacement for the dockerd daemon which implements the same API as Docker. The containers still run using runc. It is fully supported on Fedora/CentOS but has to be compiled and installed on Ubuntu.  Links: https://github.com/containers/libpod/blob/master/docs/tutorials/podman_tutorial.md, https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users/. 

k3s
: k3s is a small Kubernetes-compatible distribution, especially for small devices.


Stacks
------

The lower layers of the stack include the integration of trusted firmware, OP_TEE, bootloader (uboot?), Linux distro, CRI-O, Podman, together with the installer scripts.

On top of this part, we have the ETSI MEC API Framework, MEC APIs, and MEC application manager. This leaves out the hardware management, which is going to be complicated if we have a lot of variants of the uMEC hardware. For the Open Edge hardware, we have separated the hardware management and the upper level management -- this is the challenge that you are going to face if you will support ARM hardware in Open Edge. The functionality that needs to be abstracted is things like reboot, collecting statistics, updating firmware etc. This is what Redfish is meant for, but for very small devices Redfish could be an overkill or at least no one has yet implemented it for Raspberry Pi. The BMC is typically a small ARM server, and having a small ARM server to manage a small ARM server does not make too much sense.

So the idea would be to define a suitable subset of Redfish, or any other suitable interface, for managing the uMEC. This could be common to other edge devices also.


Components
==========

Service registries: Netflix Eureka, etcd, Consul, Apache Zookeeper

Network monitoring: logging, traceroutes, network performance





Networking
===========



Service mesh
--------------

Istio, Linkerd and Consul Connect

Network Service Mesh


Device management
=================

We will use the Salt stack for configuring the devices. Here are instructions on setting up the Salt master to managedevices:

    sudo docker pull saltstack/salt
    sudo docker run --name salt --hostname salt -P -e SALT_SHARED_SECRET=mysecretpassword -d saltstack/salt
    sudo docker exec salt "ls /srv/salt/"

In the code, this is implemented in `device_manager` directory. The Salt Stack has two modes of operation, one is the Master-Minion mode, and the other is the "salt-ssh" mode. The Master-Minion mode uses ZeroMQ for communication, so it requires some configuration to be operational. And the purpose is to configure the devices.

Salt SSH uses a file called roster 

How to put the stack together
==============================

ETSI MEC APIs
==============

ETSI MEC defines APIs using the OpenAPI (https://openapis.org) version 2. All the published APIs are in https://forge.etsi.org/rep/mec.

The API is defined in a Yaml file, and there are generators for different programming languagues in https://openapi-generator.tech. Therefore, the recommended workflow is to 

1. Download the ETSI API
2. Generate documentation and skeleton code
3. Implement the server based on the skeleton code

Open questions with this approach:
1. What 



Security architecture
======================

The overview article [Khan2017] lists many of the security requirements: 

1. Image signing and origin verification
2. Vulnerability assessment, patching and remmediation strategy
3. Reducing the attack surface
4. Secrets management store
5. Access control for containers
6. Identity federation
7. Container profiling
8. Network isolation


Interesting references
=======================

IoT device management

https://www.i-scoop.eu/internet-of-things-guide/iot-device-management/
https://www.edgexfoundry.org/blog/2017/10/25/edgex-arm64-support/


FIWARE

https://github.com/FIWARE-TMForum/Business-API-Ecosystem






References
==================

[#uMEC_RFI] Nokia µMEC SoC RFI. Link: https://nokia.sharepoint.com/:w:/r/sites/uMECplatform/Shared%20Documents/General/Nokia%20MEC/Nokia%20uMEC%20requirements/Nokia%20uMEC%20SoC%20RFI.docx?d=w12b24f13ed3f49adb6b956348567344f&csf=1&e=xZYyF0

[#uMEC_intro] µMEC introduction. Link: https://nokia-my.sharepoint.com/:p:/r/personal/tapio_tallgren_nokia_com/_layouts/15/doc2.aspx?sourcedoc={05AFBBE2-1105-45B4-9DB6-3742C5E23113}&file=uMEC%20intro.pptx&action=edit&mobileredirect=true

[#cri-o] https://cri-o.io or https://github.com/kubernetes-sigs/cri-o

[Khan2017]: Key characteristics of a Container Orchestration Platform to Enable a Modern Application. IEEE Cloud Computing, September/October 2017. 

