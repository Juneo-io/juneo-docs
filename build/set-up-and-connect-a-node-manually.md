# Set up and Connect a node manually

How to set up and connect a node to the Genesis Testnet network manually.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requierments](node-requirements.md).
{% endhint %}

{% hint style="info" %}
At the end of this guide, the JuneoGo process should remain running in the background on your server (e.g. as a service), which is not demonstrated in this guide. If you are less experienced with this and with the linux operating system in general, we suggest using our [Docker guide](set-up-and-connect-a-node-with-docker.md).
{% endhint %}

{% hint style="info" %}
For experienced linux users who wish to set up JuneoGo automatically, we suggest following the [Installation Script JuneoGo setup guide](set-up-and-connect-a-node.md).
{% endhint %}

### Setup and Connect a node to the Genesis Testnet[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

First, you should transfer the project files found [here](https://github.com/Juneo-io/juneogo-binaries) to your server. If you have [git](https://git-scm.com/) installed on your server, you may execute the following commands:

```bash
cd ~

git clone https://github.com/Juneo-io/juneogo-binaries
```

The required files will now be found in the `juneogo-binaries` folder in your home directory.

#### Configuring the initial binary files

{% hint style="warning" %}
Do not execute the _preparation.sh_ or _simple\_setup.sh_ scripts found inside the juneogo-binaries directory. These are intended to only be used when following the [Install Script JuneoGo setup guide](set-up-and-connect-a-node.md).
{% endhint %}

The binary files required to run JuneoGo are:

1. juneogo
2. jevm
3. srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e

To grant execution permissions of the binary files, please execute the following commands:

```bash
chmod +x ~/juneogo-binaries/juneogo
chmod +x ~/juneogo-binaries/plugins/jevm
chmod +x ~/juneogo-binaries/plugins/srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e
```

After this, the _juneogo_ binary should be moved to the home directory. The remaining two binaries should be moved to the `~/.juneogo/plugins` directory.

To do so, please execute the following commands:

```bash
mv ~/juneogo-binaries/juneogo ~

mkdir -p ~/.juneogo/plugins

mv ~/juneogo-binaries/plugins/jevm ~/.juneogo/plugins
mv ~/juneogo-binaries/plugins/srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e ~/.juneogo/plugins
```

The structure of your **home directory** should resemble the following:

```bash
├── juneogo
├── .juneogo/
│   ├── plugins/
│   │   └── jevm
│   │   └── srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e
```

{% hint style="warning" %}
If these files are structured differenty than above, you will not be able to connect your node.
{% endhint %}

You may now connect the node to the network by executing the juneogo binary with the following command:

```sh
./juneogo
```

This will start fetching blocks and bootstrapping your node.

{% hint style="warning" %}
Please make sure this process keeps running in the background. If the execution of the juneogo executable stops, your node will be inactive.
{% endhint %}

You may check if the node has boostrapped with the following call:

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
