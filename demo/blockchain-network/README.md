
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

### _base/peer-base.yaml_

If you look into this file, you will find a service configuration named _peer-base_ with the following contents:

```
  ...
  peer-base:
    image: fgodinho/peer:$IMAGE_TAG
    environment:
      ...
      - XSP_THREAD_POOL_SIZE=100
      - XSP_DIGEST_INVERVAL_MILLIS=1000
      - XSP_MTU=8096
      - PEER_SIGN_THRESHOLD_MTU=8096
      - CHAINCODE_INVOKE_TIMEOUT_MILLIS=100000ms
    working_dir: /
    command: ./startPeer.sh
    ...
```

If you notice closely, we have ommitted configurations related to the original Hyperledger Fabric peer node configuration. You will also notice we are using a customly-built Docker image `fgodinho/peer` and startup script `startPeer.sh` for each peer node. The necessity for the latter is that the script has to start both the Golang peer node process and a Java process for the XSPP component simultaneously.

In the `environment` section, you will find 5 custom environment variables:
* `XSP_THREAD_POOL_SIZE`: The XSPP component communicates with each peer node's Golang process via UNIX domain sockets. This flag allows you to change the number of Java threads listening on the socket on the XSPP side;
* `XSP_DIGEST_INVERVAL_MILLIS`: This is the interval in milliseconds that the Golang peer node process has to wait before sending payloads to be signed/verified to the XSPP;
* `XSP_MTU`: The maximum transmission unit for the Java socket on the XSPP side.

Note: A proper balance of the previous 3 flags is required to ensure that the XSPP is not flooded with signing/verification requests as it will cause transaction failures in doing so.

* `PEER_SIGN_THRESHOLD_MTU`: The maximum transmission unit for the socket on the peer node Golang process side;
* `CHAINCODE_INVOKE_TIMEOUT_MILLIS`: The timeout value in milliseconds to wait for a chaincode invocation before failing the corresponding transaction.

### _docker-compose.yaml_
