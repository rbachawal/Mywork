# CORTX Troubleshooting Guide

<details>
<summary>Click to know more about CORTX and its Submodules</summary>
<p>

## Introduction

CORTX is a Software-Defined Storage that is Simple, Efficient, and Secure and lets you store about 1 Peta Byte of data for costs as low as $50K per Peta Byte. CORTX provides unified management of Storage, a generic S3 Interface, and all the standard features of S3 Storage (such as multiple S3 Accounts, IAM users, and S3 buckets with access policies). 

CORTX is a perfect solution to leverage your customized innovation by efficiently managing exponential data growth. 

## CORTX Submodules 

The CORTX software consists of the following submodules

### Provisioner 

Cortx Provisioner is a collection of Salt formulate and utility scripts, that assist you to set up your VM and Hardware environment(s) with components in Cortx stack and their dependencies in well orchestrated way.

### Motr 

The Motr submodule is responsible for storing and managing data on an enclosure and underlying disks. 

We've listed the features of the Motr submodule : 

- Scalable:
  - Horizontal scalability: grow your system by adding more nodes. The Motr submodule is designed for horizontal scalability with no meta-data hotspots, shared-nothing IO paths and extensions running on additional nodes.
  - Vertical scalability: with more memory and CPU on the nodes.
- Fault-tolerant: with flexible erasure coding that takes hardware and network topology into account.
- Fast network raid repairs.
- Observable: with built-in monitoring that collects detailed information about the system behavior.
- Extensible. 
- Extension interface.
- Flexible transactions.
- Open source.
- Portable: runs in user space on any version of Linux.

### High Availability (HA) 

The High Availability submodule or HA is responsible for ensuring that CORTX Solution is available in case of any hardware component or software service failures. It takes care of failover/ failback control flow of the affected services and stabilizes them across the CORTX cluster. 

### Simple Storage Service (S3) 

The Simple Storage Service or S3, is an object storage service that offers industry-leading scalability, data availability, security, and performance. Once you create an S3 Account or User, you will need to create a 'Bucket' within S3 service. Bucket(s) are used to store object-based files and are similar to folders. You can upload your files (objects) to or download them from bucket(s).

:page_with_curl: **Note:** The S3 submodule has a dependency on Provisioner, HA, and Motr submodules.

### CORTX Manager (CSM) 

The Cortx Manager or CSM, is the Graphical User Interface for CORTX and is a part of Cortx Management network. The Cortx Manager interacts with various other Cortx submodules to help you manage the system. While the Cortx Manager is present on both nodes, it is active only on one node at a time. If the Cortx manager fails to run on one node, it is automatically activated on the other node.  

The Cortx Manager comes powered with the following features:  

- S3 account management 
- System health monitoring 
- System alert display

### CORTX Monitor  

CORTX Monitor tracks platform health and raises alerts on sensing any unintended state. It can detect hardware faults, removals or replacements by continuously sensing sub-systems like Storage Enclosure, Node Servers and components, and network interfaces.


</p>
</details>
