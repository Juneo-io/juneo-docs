---
description: Powering L1 networks with solid foundations, from Avalanche to Juneo
---

# Avalanche Foundations

Avalanche is a blockchain platform that enables the creation of sovereign **Layer 1 (L1) networks**, each designed for specific applications. It uses the **Snowman++** proof-of-stake consensus protocol, which supports fast finality and low latency, processing and verifying transactions in under 2 seconds. Its native token is **AVAX**.

***

### L1 Networks

An L1 Network is a fully sovereign blockchain network that defines its own rules for membership, tokenomics, and governance. Each L1 network is validated by a dynamic set of validators from the Avalanche network, ensuring consensus on one or more blockchains within the L1. While each Layer 1 validates its own set of blockchains, one L1 network can validate multiple blockchains.

<figure><img src="../.gitbook/assets/Screenshot 2025-03-27 230433.png" alt=""><figcaption></figcaption></figure>

The implementation of L1 Networks offers many benefits:

* The performance of each L1 is independent, meaning that the activity on one L1 won’t affect others
* L1 creators have complete flexibility to define tokenomics, validator requirements, usage incentives, and other configurations specific to their L1
* Validators can choose which L1s they wish to validate, tailoring their participation to their interests

Every validator that wants to validate a L1 Network must be a Validator of the Primary Network.

***

### Primary Network

The Primary Network is a special Layer 1 that orchestrates several blockchains, each serving different purposes:

* The **Platform Chain** (P-Chain), equivalent to the **P-Chain** on Juneo Supernet
* The **Contract Chain** (C-Chain), equivalent to the **JUNE-Chain** on Juneo Supernet
* The **Exchange Chain** (X–Chain), equivalent to the **JVM-Chain** on Juneo Supernet



<figure><img src="../.gitbook/assets/Screenshot 2025-03-27 225732.png" alt=""><figcaption></figcaption></figure>

The **P-Chain** handles all validator and L1 Networks  operations. Its API supports the creation of new blockchains and L1 Networks, the addition of validators to L1 Networks, staking operations, and other platform-level operations.

The **C-Chain** (equivalent to the **June-Chain**) is an implementation of the Ethereum Virtual Machine (EVM). Its API supports Geth's API and supports the deployment and execution of smart contracts written in Solidity. This chain is an instance of the Coreth Virtual Machine.

The **X-Chain** (or **JVM-Chain**) is responsible for operations on digital smart assets known as Native Tokens. A smart asset is a representation of a real-world resource (for example, equity, or a bond) with sets of rules that govern its behaviour, like "can’t be traded until tomorrow." This chain’s API supports the creation and trade of Native Tokens.

***

### Virtual machine

A Virtual Machine (VM) is a blueprint for a blockchain. It runs on all nodes that are validating a specific L1 Network. VM’s generally define transactions that are executed and how blocks are created. Virtual Machines deal with blocks and state. A VM’s functionality is to:

* Define the representation of a blockchain's state
* Represent the operations in that state
* Apply the operations in that state

On Avalanche, builders can deploy a custom virtual machine on their Layer 1 Network.
