# Set up and Connect a node using the Install Script

How to set up and connect a node to the Socotra Testnet v1 network using the installation script.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requierments](node-requirements.md).
{% endhint %}

### Setup and Connect a node to the Socotra Testnet v1[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

The install script which will be used to configure and run your node can be found [here](https://github.com/Juneo-io/juneogo-binaries/blob/main/simple\_setup.sh).

This script is intended as a convenient way to configure JuneoGo and run it as a service. This script is not recommended for production environments. Before running this script, make yourself familiar with its contents as well as potential risks and limitations.

{% hint style="info" %}
This install script assumes:

* JuneoGo is not running and not already installed as a service
* You have Git installed on your system
{% endhint %}

{% hint style="info" %}
If you are using a different distribution of Linux other than Ubuntu, this script may not work as it assumes that `systemd` is used to run system services. It will likely work on systems that use `systemd` to run services, but it has been developed and tested on Ubuntu.
{% endhint %}

To run the script, please execute the following command:

```bash
wget -O - https://raw.githubusercontent.com/Juneo-io/juneogo-binaries/main/simple_setup.sh | bash
```

This will clone the juneogo-binaries repository from github, configure all necessary files, and create a service which will run JuneoGo and start bootstrapping your node.

You may check if your node has boostrapped with the following call:

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
