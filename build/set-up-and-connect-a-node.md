# Set up and Connect a node

How to set up and connect a node to the Socotra Testnet v1 network.

### Requirements

#### Computer Hardware and OS <a href="#computer-hardware-and-os" id="computer-hardware-and-os"></a>

Juneogo is a lightweight protocol. Note that as network usage increases, hardware requirements may change.

* CPU: 4 cores
* RAM: 8 GiB
* Storage: 160 GiB SSD (500 GiB SSD for Mainnet)
* OS: Ubuntu 20.04/ 22.04

#### Networking[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#networking) <a href="#networking" id="networking"></a>

To run successfully, juneogo needs to accept connections from the Internet on the network ports `9650` and `9651`. Before you proceed with the installation, you need to determine the networking environment your node will run in.

#### **Running on a Cloud Provider**[**​**](https://docs.avax.network/nodes/build/run-avalanche-node-manually#running-on-a-cloud-provider)

If your node is running on a cloud provider computer instance, it will have a static IP. Find out what that static IP is, or set it up if you didn't already.

#### **Running on a Home Connection**[**​**](https://docs.avax.network/nodes/build/run-avalanche-node-manually#running-on-a-home-connection)

If you're running a node on a computer that is on a residential internet connection, you have a dynamic IP (meaning your IP will change periodically). You will need to set up inbound port forwarding of port `9651` from the internet to the computer the node is installed on.

### Setup and Connect a node to the Socotra Testnet v1[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

For this step, you will have to download the binary files found [here](https://github.com/Juneo-io/juneogo-binaries). If you have [git](https://git-scm.com/) installed on your system, please execute the following in your command line:

`git clone https://github.com/Juneo-io/juneogo-binaries`

Your binaries will be found in the `juneogo-binaries` folder on your system.

#### Configuring the initial binary files

The first step is to transfer the required binaries to the **home directory** of the server on which you wish to run your node. These binaries are:

1. juneogo
2. jevm

{% hint style="info" %}
The Supernet binary, _srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e_, is not required for this step. It will be required if you wish to deploy a VM on your Supernet.
{% endhint %}

After moving these binaries to the server, we next step is to allow execution permissions for them with the following commands:

```bash
chmod +x juneogo
chmod +x jevm
```

Next, the directory `.juneogo/plugins` should be created in the home directory of our server,  and the _jevm_ binary should be placed there:

```bash
mkdir -p .juneogo/plugins
mv jevm .juneogo/plugins
```

The structure of your home directory should resemble the following:

```bash
├── juneogo
├── .juneogo/
│   ├── plugins/
│   │   └── jevm
```

{% hint style="info" %}
If these files are structured differenty than above, you will not be able to connect your node.
{% endhint %}

You may now connect the node to the Socotra network by executing the juneogo binary with the following command:

```sh
./juneogo
```

This will start fetching blocks and bootstrapping your node.&#x20;

{% hint style="warning" %}
Please make sure this process keeps running in the background. If the execution of the juneogo executable stops, your node will be inactive.
{% endhint %}

You may check if the node has is boostrapped with the following call:

```sh
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"JUNE"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

Example response:

```
{
  "jsonrpc": "2.0",
  "result": {
    "isBootstrapped": true
  },
  "id": 1
}
```

After the bootstrapping process has completed, you may proceed to the next step - [add a node to the Validator Set](../validate/add-a-validator.md).
