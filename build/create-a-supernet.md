---
description: Creating a Layer-1 Network (which will allow us to later deploy our own VM)
---

# Create a Layer-1 Network

{% hint style="info" %}
A prerequisite for this tutorial is that you have a running node that is validating the primary network, for which the steps can find in the [Add a Juneo Supernet validator node ](../validate/add-a-validator.md)guide.
{% endhint %}

We will be using example code found in the [juneojs-examples ](https://github.com/Juneo-io/juneojs-examples)repository to deploy our Layer-1 Blockchain (which uses the library [juneojs](https://www.npmjs.com/package/juneojs)).

{% hint style="info" %}
We recommend using JuneoJS on a system where you have access to a code editor.
{% endhint %}

### Create a Layer-1 Network

First, please make sure that you have the latest version of [NodeJS](https://nodejs.org/en) installed on your system.

{% hint style="warning" %}
If the version of NodeJS on your system is not up to date with the LTS version found at [https://nodejs.org](https://nodejs.org/en), the scripts in this guide will not be able to execute.
{% endhint %}

You will also need have the npm package[ ts-node](https://www.npmjs.com/package/ts-node) installed, which will be used to execute the sample code provided in JuneoJS.

After verifying the information above, please download the [juneojs-examples](https://github.com/Juneo-io/juneojs-examples) repository to your local machine. If you have [git](https://git-scm.com/) installed on your system, please execute the following in your command line:

```bash
git clone https://github.com/Juneo-io/juneojs-examples
```

The required files will be found in the directory `juneojs-examples`.

Next, please open `juneojs-examples` in a command line and execute the following command:

```bash
npm install
```

After this, open `juneojs-examples` in a code editor of your choice, create a .env file from the provided .env.example, and paste your mnemonic phrase in the MNEMONIC variable. Example:

```sh
MNEMONIC="raven whip pave toy benefit moment twin acid wasp satisfy crash april"
```

To perform the transactions required to deploy a Layer-1 Network, you will need to have funds on the P-chain.  In this tutorial, we will first be crossing assets from the JUNE-chain to the JVM-chain, and then from  the JVM-chain to the P-chain.

In a command line window open in the root of the JuneoJS library, execute the following:

```bash
npx ts-node ./src/docs/crossJUNEtoJVM.ts
```

This will cross 1.1 JUNE from the JUNE-chain to the JVM-chain. After this, we will cross 1 JUNE from the JVM-chain to the P-chain by executing the following command:

```bash
npx ts-node ./src/docs/crossJVMtoP.ts
```

The next step is the L1 Network creation. Please execute the following in the command line:

```
npx ts-node ./src/supernet/createSupernet.ts
```

This will produce an output to the terminal containing your supernetID. Example: _ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwh._

This is the id of the transaction that has created your L1, and is the id of the Layer-1 Network as well. Please save it as we will needed in the following steps.

### Add a validator to the Layer-1 Network

The next step is to perform the addSupernetValidator transaction.

{% hint style="info" %}
If your L1 has no validators, all blockchains in that Layer-1 will be inactive and will not be able to process transactions.
{% endhint %}

Please open the file `./src/supernet/addSupernetValidator.ts` in your code editor, and update the following variables to contain the correct values. Example:

```typescript
const nodeId: string = 'NodeID-B2GHMQ8GF6FyrvmPUX6miaGeuVLH9UwHr'
const supernetId: string = 'ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP'
const durationInDays: number = 20 // number of days you will validate your Supernet
```

We will keep the **durationInDays** variable the same. However, you may update it to a diffent value.

{% hint style="info" %}
The variable **durationInDays** must be less than the amount of remaining days your node will be validating the Primary network.

If your node has 21 days (or a little over 20 days) left of Primary Network validation, you must set **durationInDays** to 20 or less.
{% endhint %}

Then, execute this file:

```sh
npx ts-node ./src/supernet/addSupernetValidator.ts
```

This will add our node as a validator for our Layer-1 Network. However, we need to perform an additional step before our node can begin validating our L1. First, please stop your node.



For users following the [manual ](set-up-and-connect-a-node-manually.md)guide for setting up juneogo: please create a file titled `config.json` in the home directory.&#x20;

For users following the [install script](set-up-and-connect-a-node.md) guide for setting up juneogo: your `config.json` can already be found in the home directory.&#x20;

For users who are running juneogo using [juneogo-docker](https://github.com/Juneo-io/juneogo-docker): your config file can be found in the `juneogo-docker/juneogo/.juneogo/config.json` directory.



In your `config.json` file, please include the configuration flag `track-supernets,` specifying the L1s you want your node to track (in this case, the Layer-1 you have just created).

Example:

```json
{
 "track-supernets":"ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP"
}
```

After saving the file, you may run your node again.&#x20;



For users following the [manual ](set-up-and-connect-a-node-manually.md)juneogo setup guide, please execute the following :

```bash
./juneogo --config-file="./config.json"
```

For users following the [install script](set-up-and-connect-a-node.md) juneogo setup guide, please restart the juneogo service:

```bash
systemctl --user restart juneogo.service
```

For users running juneogo using [juneogo-docker](https://github.com/Juneo-io/juneogo-docker), please re-start your node using docker compose.



Your node is now tracking the Layer-1 Network you have just created.&#x20;

{% hint style="info" %}
If **your node** is the only node validating your L1 - once your node stops validating it, all chains within it will not be able to process transactions.

You would have to re-perform the **Add** Supernet **Validator** transaction for your Layer-1 Network to process transactions again.
{% endhint %}

You may now proceed to the next step: [Deploy a VM](deploy-a-vm.md).
