---
description: Seamlessly transition your node and assets to the mainnet
---

# Migrate from Socotra Testnet to Juneo Mainnet

{% hint style="warning" %}
CAUTION : Updating your node to mainnet will make your chains and supernets created on Socotra inaccessible.
{% endhint %}

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
Carefully backup your Node if you participated to incentivised testnet! Otherwise you can loose your allocation. ( You only need to backup the Node ID )
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
Use only this command if you have followed our [Install Script](migrate-from-socotra-testnet-to-juneo-mainnet.md#b.-running-the-node-using-the-install-script)
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
