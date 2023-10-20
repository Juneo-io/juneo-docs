# Avalanche foundations

Avalanche is a blockchain platform that allows separate chains to be created for different applications. It uses the Snowman++ proof-of-stake consensus protocol which supports quick finality and low latency, allowing transactions to be processed and verified within 2 seconds. Its native token is AVAX.

### Subnets

A Subnet is a sovereign network which defines its own rules regarding its membership and token economics. It is composed of a dynamic subset of Avalanche validators working together to achieve consensus on the state of one or more blockchains. Each blockchain is validated by exactly one Subnet, while a Subnet can validate many blockchains. On Juneo, these sovereign networks are known as Supernets.

The implementation of Subnets offers many benefits:

* The performance of a single Subnet is isolated from that of another, meaning that increased usage of one Subnet will not affect the other.
* Subnet creators can freely determine the token economics, validator requirements, usage incentives and application specific configuration for their Subnet
* Validators can choose which Subnets they are interested in validating

Every validator that wants to validate a Subnet must be a Validator of the Primary Network.

### Primary Network

The Primary Network is a special Subnet that runs three blockchains:

* The Platform Chain (P-Chain), equivalent to the P-Chain on Juneo
* The Contract Chain (C-Chain), equivalent to the JUNE-Chain on Juneo
* The Exchange Chain (X–Chain), equivalent to the JVM-Chain on Juneo



The **P-Chain** handles all validator and Subnet-level operations. Its API supports the creation of new blockchains and Subnets, the addition of validators to Subnets, staking operations, and other platform-level operations.

The **C-Chain** (equivalent to the **June-Chain**) is an implementation of the Ethereum Virtual Machine (EVM). Its API supports Geth's API and supports the deployment and execution of smart contracts written in Solidity. This chain is an instance of the Coreth Virtual Machine.

The **X-Chain** (or **JVM-Chain**) is responsible for operations on digital smart assets known as Native Tokens. A smart asset is a representation of a real-world resource (for example, equity, or a bond) with sets of rules that govern its behaviour, like "can’t be traded until tomorrow." This chain’s API supports the creation and trade of Native Tokens.

One asset traded on the X-Chain is AVAX. When you issue a transaction to a blockchain on Avalanche, you pay a fee denominated in AVAX.

The X-Chain is an instance of the Avalanche Virtual Machine (AVM).

### Virtual machine

A Virtual Machine (VM) is a blueprint for a blockchain. It runs on all nodes that are validating a specific Subnet. VM’s generally define transactions that are executed and how blocks are created. Virtual Machines deal with blocks and state. A VM’s functionality is to:

* Define the representation of a blockchain's state
* Represent the operations in that state
* Apply the operations in that state

On Avalanche, builders can deploy a custom virtual machine on their Subnet.
