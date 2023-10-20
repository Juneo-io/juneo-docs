# Nodes

All nodes are set up and connected to the network using pre-compiled [juneogo executable files](https://github.com/Juneo-io/juneogo-binaries). In later stages of the project, the juneogo protocol will be open source.



## Node types

Depending on its configuration, a node can be of the following types:



### **Regular Node**

This node can access blockchain data and propose transactions through RPC calls. However, it is not a validator of the Primary network or any Supernets.

### API Node

An API Node is the same as a Regular node, however it supports RPC calls from remote machines.

### Validator Node

Validator nodes have all of the capabilities of a Regular Node/ API node, but they also participate in the network's consensus mechanism by validating transactions.

### Archive Node

An archive node has database pruning disabled for certain chains it is validating, meaning it will have archive data of all historical states of those chains.

{% hint style="info" %}
A node can be several types simultaneously - an **Archive Node** for one chain it is validating, but a **Validator node** for another chain.
{% endhint %}

For more details about the Network Architecture, see the [Juneo Litepaper](https://juneo.com/pdf/JUNEO%20Litepaper%20\(vs%201.07\).pdf) or visit [juneo.com](https://juneo.com/).



## Node Operations Reference

### Build

<table data-header-hidden><thead><tr><th></th><th></th><th data-hidden></th></tr></thead><tbody><tr><td><a href="../build/set-up-and-connect-a-node.md">Set up and Connect a node manually</a></td><td>How to set up and connect to the Socotra Testnet v1 network without the installer script</td><td></td></tr><tr><td><a href="../build/create-a-supernet.md">Create a Supernet</a></td><td>How to create a Supernet on the Socotra Testnet v1</td><td></td></tr><tr><td><a href="../build/deploy-a-vm.md">Deploy a VM</a></td><td>How to deploy a VM on your Supernet</td><td></td></tr></tbody></table>

### Maintain

| [Node Backup and Restore](../maintain/node-backup-and-restore.md) | How to backup important files to be able to restore a node. |
| ----------------------------------------------------------------- | ----------------------------------------------------------- |

### Validate

<table data-header-hidden><thead><tr><th></th><th></th><th data-hidden></th></tr></thead><tbody><tr><td><a href="../validate/add-a-validator.md">Add a node to the Validator set</a></td><td>How to add a node to the Validator Set</td><td></td></tr></tbody></table>
