
# Blockchain Network Configuration

This blockchain middleware is extended from the Hyperledger Fabric platform, version 1.1. Most of the Docker configurations that can be performed here are inherited from the original platform. Thus, this README shall mostly refer to configurations relevant to the extended services we added to Fabric. For help with Fabric's configurations, please [click here](https://hyperledger-fabric.readthedocs.io/en/release-1.1/).

Additionally, please refer to the main README of this repository for instructions regarding startup or shutdown scripts.

## Network configuration file structure

Before we explain on the specific configurations of our system, let us briefly explain how the blockchain network file structure is set up:

* _docker-compose.yaml_: Entry point for Docker Compose defining the network topology in terms of peer nodes and orderer nodes, and a CLI to manage the network;
* _docker-compose-couch.yaml_: CouchDB container configuration for each and every peer node;
* _docker-compose-bftsmart.yaml_: BFT-SMaRt ordering service configuration that can be loaded if the user chooses a BFT consensus;
* _docker-compose-kafka.yaml_: Kafka and Zookeeper ordering service configuration that can be loaded if the user chooses a non-BFT consensus;
* _base/docker-compose-base.yaml_: Core configuration file specifying specific parameters for each peer node and orderer node of the network;
* _base/docker-compose-bftsmart-base.yaml_: Common configurations for the BFT-SMaRt ordering service;
* _base/docker-compose-kafka-base.yaml_: Common configurations for the Kafka and Zookeeper ordering service;
* _base/orderer-base.yaml_: Common configurations for orderer nodes;
* _base/peer-base.yaml_: Common configurations for peer nodes;
* _crypto-config.yaml_: Configuration file required for generating any needed cryptographic material for the network; 
* _configtx.yaml_: Configuration file required for generating Fabric's channels and genesis block. 

## Extended services configuration
