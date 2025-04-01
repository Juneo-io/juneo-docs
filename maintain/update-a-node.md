---
description: >-
  Regularly updating your node is essential for maintaining optimal performance,
  security, and network compatibility.
---

# Update a Node

This guide outlines the steps to update your node, whether you are running it manually, via an installation script, or using Docker. The process includes stopping your node, backing up important data, updating the binaries, and restarting the node to ensure its stays syncronized with the Juneo Mainnet. Follow each step carefully to preserve the integrity and connectivity of your node.

## 1. Stop your node

Depending on how you run your instance you should stop it, accordingly.&#x20;

### a. Running the node manually

Access the window where you are running the binary `./juneogo` and stop it with Ctrl+c.

### b. Running the node using the Install Script

As you run your Node as service you need to stop it with `systemctl`.

```sh
sudo systemctl stop juneogo
```

### c. Running with juneogo-docker

You can stop your docker with the following:&#x20;

1\) Go to `juneogo-docker`

2\) Run `docker-compose down`

## 2. Backup your node

{% hint style="danger" %}
Carefully backup your Node if you are curently validating the Juneo Mainnet Network. ( You only need to backup the Node ID )
{% endhint %}

{% hint style="warning" %}
It's recommended to save your staking files in a different location than `juneogo` or `juneogo-docker` directory.
{% endhint %}

Follow the guide [node-backup-and-restore.md](node-backup-and-restore.md "mention") and if you have any issues or question reach us on the [Official Discord](https://discord.com/invite/juneo).

## 3. Update binaries

{% hint style="danger" %}
DANGER : Be sure to backup your node BEFORE doing the next steps.&#x20;
{% endhint %}

### a. Running the node manually

Go to the github `juneogo-binaries` repository and download the latest version of these files:&#x20;

```
- juneogo
- jevm
```

Connect to your server through a SFTP agent and copy paste the new files.

To grant execution permissions of the binary files, please execute the following commands:

```bash
chmod +x ~/YOUR-PATH/juneogo
chmod +x ~/YOUR-PATH/plugins/jevm
```

### b. Running the node using the Install Script

{% hint style="danger" %}
Use only this command if you have followed our [Install Script](update-a-node.md#b.-running-the-node-using-the-install-script)
{% endhint %}

Use the following command on your server :&#x20;

> {% code overflow="wrap" %}
> ```bash
> bash <(wget -qO- https://raw.githubusercontent.com/Juneo-io/juneogo-binaries/main/update.sh)
> ```
> {% endcode %}

### c. Running with juneogo-docker

Go to the Juneogo directory: `juneogo-docker` and run these commands:

```bash
git checkout origin/main ./juneogo/juneogo
git checkout origin/main ./juneogo/.juneogo/plugins/jevm
```

{% hint style="info" %}
If it doesn't work you can try to run `git stash` or to clone it again by following Set up steps.&#x20;
{% endhint %}

## 4. Restart your node

By running your node as usual you will now bootsrap the Mainnet.

You can check that your node is bootsrapped through the regular curl request.&#x20;

## 5. Checking Your Node's Version Status <a href="#user-content-4-boostrapping-status" id="user-content-4-boostrapping-status"></a>

You can check the current version of your node by using the following RPC call:

### a. For nodes running manually or via the Install Script:

```bash
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeVersion"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

### b. For nodes running with juneogo-docker:

```bash
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeVersion"
}' -H 'content-type:application/json;' 192.168.10.2:9650/ext/info
```

Example Response:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "version": "juneogo/1.1.0",
    "databaseVersion": "v1.4.5",
    "rpcProtocolVersion": "35",
    "gitCommit": "",
    "vmVersions": {
      "jevm": "v1.1.0",
      "jvm": "v1.1.0",
      "platform": "v1.1.0",
      "srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e": "v0.6.4"
    }
  },
  "id": 1
}

```
