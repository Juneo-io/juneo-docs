# Set up and Connect a node using the Install Script

How to set up and connect a node to the Mainnet using the installation script.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requirements](node-requirements.md).
{% endhint %}

{% hint style="info" %}
If you are less with the linux operating system, we suggest using our [Docker guide](set-up-and-connect-a-node-with-docker.md).
{% endhint %}

### Setup and Connect a node to the Mainnet <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

The install scripts which will be used to configure and run your node can be found [here](https://github.com/Juneo-io/juneogo-binaries). We will be using two script files - `preparation.sh` and `simple_setup.sh`.

These scripts are intended as a convenient way to configure JuneoGo and run it as a service. These scripts are not recommended for production environments. Before executing them, make yourself familiar with their contents as well as potential risks and limitations.

{% hint style="info" %}
This install scripts assume:

* JuneoGo is not running and not already installed as a service
* You have Git installed on your system
{% endhint %}

{% hint style="info" %}
If you are using a different distribution of Linux other than Ubuntu, these scripts may not work as they assume that `systemd` is used to run system services. They will likely work on systems that use `systemd` to run services, but they have been developed for and tested on Ubuntu.
{% endhint %}

The first step is to run the `preparation.sh` script.

{% hint style="warning" %}
CAUTION:

Before executing this script, make sure you understand the implications of disabling root login and password authentication to avoid unexpected errors and security issues.

During script execution, read all the prompts carefully and make sure you understand them before proceeding further.
{% endhint %}

Please execute the following command as root (or with sudo privileges):

```bash
bash <(wget -qO- https://raw.githubusercontent.com/Juneo-io/juneogo-binaries/main/preparation.sh)
```

Executing this script will create a new user`juneogo` on your server, which will be running your juneogo node.

The next step is to logout of your server, and then login as the user `juneogo`. Once you have logged in as the user `juneogo`, you may execute the `simple_setup.sh` script with the following command:

```bash
bash <(wget -qO- https://raw.githubusercontent.com/Juneo-io/juneogo-binaries/main/simple_setup.sh)
```

This will clone the juneogo-binaries repository from github, configure all the necessary files, and create a service which will run JuneoGo and start bootstrapping your node.

If you want to connect the node to the Socotra testnet, you need to update the `config.json` file with the `network-id` flag :&#x20;

{% hint style="warning" %}
If you want to connect the node to the Socotra testnet, you need to update the `config.json` file with the `network-id` flag :&#x20;

```bash
{
  "network-id":"socotra"
}
```
{% endhint %}

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
