# Set up and Connect a node manually

How to set up and connect a node to the Socotra Testnet v1 network manually.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requierments](node-requirements.md).
{% endhint %}

### Setup and Connect a node to the Socotra Testnet v1[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

For this step, you will have to download the binary files found [here](https://github.com/Juneo-io/juneogo-binaries). If you have [git](https://git-scm.com/) installed on your system, please execute the following in your command line:

`git clone https://github.com/Juneo-io/juneogo-binaries`

Your binaries will be found in the `juneogo-binaries` folder on your system.

#### Configuring the initial binary files

The first step is to transfer the required binaries to the **home directory** of the server on which you wish to run your node. These binaries are:

1. juneogo
2. jevm
3. srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e

After moving these binaries to the server, we next step is to allow execution permissions for them with the following commands:

```bash
chmod +x juneogo
chmod +x jevm
chmod +x srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e
```

Next, the directory `.juneogo/plugins` should be created in the home directory of our server,  and the _jevm_ binary should be placed there:

```bash
mkdir -p .juneogo/plugins
mv jevm .juneogo/plugins
mv srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e .juneogo/plugins
```

The structure of your home directory should resemble the following:

```bash
├── juneogo
├── .juneogo/
│   ├── plugins/
│   │   └── jevm
│   │   └── srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e
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