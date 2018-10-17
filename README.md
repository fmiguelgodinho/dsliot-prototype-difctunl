# Prototype for a Decentralized and Scalable Ledgering system for the Internet-of-Things (IoT)

This repository hosts the codebase for the prototype that was built for an MSc. Thesis, whose purpose is to provide a decentralized and scalable middleware for IoT environments using blockchain technology.

## Context

The Internet-of-Things (IoT) is simultaneously the largest and the fastest growing distributed system known to date. With the expectation of 50 billion of devices coming online by 2020, far surpassing the size of the human population, problems related to scale, trustability and security are anticipated. Current IoT architectures are inherently flawed as they are centralized on the cloud and explore fragile trust-based relationships over a plethora of loosely integrated devices, leading to IoT platforms being non-robust for every party involved and unable to scale properly in the near future. The need for a new architecture that addresses these concerns is urgent as the IoT is progressively more ubiquitous, pervasive and demanding regarding the integration of devices and processing of data increasingly susceptible to reliability and security issues.

Thus, we propose a decentralized ledgering solution for the IoT, leveraging a recent concept: blockchains. Rather than replacing the cloud, our solution presents a scalable and fault-tolerant middleware for recording transactions between peers, under verifiable and decentralized trustability assumptions and authentication guarantees for IoT devices, cloud services and users. Following on the emergent trend in modern IoT architectures, we leverage smart hubs as blockchain gateways, aggregating, pre-processing and forwarding small amounts of data and transactions in proximity conditions, that will be verified and processed as transactions in the blockchain. The proposed middleware acts as a secure ledger and establishes private channels between peers, requiring transactions in the blockchain to be signed using threshold signature schemes and group oriented verification properties. The approach improves the decentralization and robustness characteristics under Byzantine fault-tolerance settings, while preserving the blockchain distributed nature.

## Architecture

The following Figure illustrates the architecture of the prototype:

<p align="center">
  <img src="proto-arch.png" width="600" title="Prototype Architecture Overview">
</p>

The prototype is modeled in a tiered architecture composed by three different layers. These represent different software services and components:
* On the lower level, are the blockchain-enabled services, leveraged by an _extended_ version of the Hyperledger Fabric platform, where the support for interchangeable threshold and group multi-signatures was added, integrated with extended consensus plane services for the support of Byzantine fault-tolerant properties;
* On the center, is the materialization of the concept of smart hubs, responsible for the intermediation of interactions from user devices and IoT devices (regarded as clients of the provided services in the blockchain-enabled architecture), and forwarding those operations as transactions with data-management functions enabled by the backed blockchain services;
* On the upper and final level, we have the actual clients of the service that could be found in regular IoT environments (_things_ or IoT devices, and end users).

In terms of technologies and communication protocols, the bottom layer inherits from Hyperledger Fabric the gRPC protocol for communication between blockchain nodes and the Golang codebase. The smart hub layer was implemented in Java, using a slightly modified version of the Hyperledger Fabric SDK and harnessing CoAP and HTTP protocols for communication with client entities (using the CoAP library by https://github.com/ARMmbed/java-coap and the lightweight Java Spark framework which comes with an integrated Jetty HTTP server http://sparkjava.com/). Further, this layer uses MongoDB for caching data. For the final layer, we provide a test Java CoAP client implementation and an Android client.

## Pre-requisites

## Instructions

The prototype's software artifacts can be found in the `demo` folder.


### 1. Starting a Blockchain Services network

This first set of steps will start a bootstrap Docker virtual network for the blockchain services layer of the prototype. As it requires the usage of multiple Docker containers simultaneously for emulating blockchain nodes and their inner services and components, it is recommended that this is executed on a high-end machine. It can also be done in a distributed setting upon configuration of the Docker Compose YAML files. However, the configurations we provide are generically set up for a virtual distributed environment within a single physical machine.

1. Go into the blockchain services network folder: `cd demo/blockchain-network`. Within this folder you will find a few YAML files, some subfolders and a set of bash scripts: `generate.sh`, `start.sh`, `stop.sh`, `kill.sh`. These scripts are responsible for generating the genesis block and any needed cryptographic material for node communication, membership and identification, for starting an instance of the blockchain network and all underlying services and nodes, for stopping the network temporarily, and for _killing_ the network as whole by clearing all its resources. 
2. 


## Source-code


## Contact us

For any questions or suggestions you may have, please get in touch with us:
* Francisco Godinho, f.godinho@campus.fct.unl.pt
* Henrique Domingos, hj@fct.unl.pt


