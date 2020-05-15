# Introduction

Details about additional tests required for this Blueprint in addition to the Akraino Validation Feature Project

Akraino Test Group Information

The Testing Ecosystem

Testing Working Group Resources

# Overall Test Architecture

The software stack can  be tested with a virtual machine. The Vagrant version is currently work in progress. 

The installation system uses a server for booting and hosting the root file system. This allows a rapid turnaround to testing, and it also saves the MMC from wear and tear. The test server can be the same server as the boot server, but for clarity we are drawing it as separate.

The system can be deployed as a standalone or as a cluster, and with different hardware. This document describes the standalone system.

![Test architecture](test_env.svg)


# Test API description

## Akraino common tests


## Blueprint extension tests

The ETSI MEC-11 implementation provides a REST interface. Writing a tests suite for it is work in progress.


## BluVal Tests

The µMEC installs k3s which is a lightweight version of kubernetes. Therefore, all the Akraino Bluprint Validation tests are applicable:

- Vuls
- kube-hunter
- Kubernetes conformance
	
## Additional Testing

## Bottlenecks/Errata
