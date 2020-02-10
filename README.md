# MicroMEC Documentation

Welcome to the MicroMEC Documentation repository. The following documentation 
are located here:

* [Architecture](architecture/umec_architecture.md)
* [Installation](installation/umec_installation.md)
* [Testing](testing/umec_testing.md)
* Release 3

Here at github the project hosts the most relevant documentation, API source, 
web site content and important information for web admins. We will also describe
additional community services and list the available demos of the project.

## Introduction

MicroMEC (aka Micro-MEC, uMEC) is part of Linux Foundation's [Akraino Edge Stack](https://wiki.akraino.org/display/AK/Micro-MEC).

MEC stands for Multi-access Edge Computing. MEC is meant to enable new 
functionalities and business models on the network edge. Running applications on
the network edge mean:

* lower latency for end users
* real time access to radio network information
* less load on the mobile core network
* improved security and privacy

uMEC targets low powered devices with a maximum power consumption of 30W. It 
supports different sensors and data sources, such as cameras. 

The main operational mode for μMEC is collecting information, processing and
forwarding the information, if and when it is necessary.

μMEC can also provide services to local users and show information on the 
display of the end user's device.

μMEC supports a set of ETSI MEC standard APIs: 

 * [MEC011 Platform Application Enablement](https://forge.etsi.org/rep/mec/gs011-app-enablement-api) 

The [Akraino MicroMEC pages](https://wiki.akraino.org/display/AK/Micro-MEC)
provide additional information about the project. 

## uMEC Concept in a Nutshell

μMEC is a small form factor hardware and software platform especially designed 
for Smart City services on the ultra far edge. uMEC devices can be installed on
light poles, buildings, vehicles using 5G, WLAN or fiber connections.

The uMEC APIs and the applications that implement (ie. use) the APIs are 
containerized, and orchestrated by suitable systems such as k3s. 

Data from sensors and other sources are obtained via various interfaces that are 
often vendor specific. These implementations are in form of plugins that connect
to the external devices and convert their data for further use. The data is 
pushed to a messaging system. The APIs will provide access to the data via this 
messaging system. The same system will enable control of the devices where 
applicable. More on this can be found in the [Architecture](/architecture/umec_architecture.md) specification. 


