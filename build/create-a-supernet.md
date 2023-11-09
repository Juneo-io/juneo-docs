# Create a Supernet

Creating a Supernet (which will allow us to later deploy our own VM).

{% hint style="info" %}
A prerequisite for this tutorial is that you have a running node that is validating the primary network, for which the steps can find in the [Add a JUNEO validator node ](../validate/add-a-validator.md)guide.
{% endhint %}

We will be using example code found in the [juneojs-examples ](https://github.com/Juneo-io/juneojs-examples)repository to deploy our Supernet (which uses the library [juneojs](https://www.npmjs.com/package/juneojs)).

{% hint style="info" %}
We recommend using JuneoJS on a system where you have access to a code editor.\
\
Please make sure that you have [Node.JS](https://nodejs.org/en) installed on this system, along with the npm package[ ts-node](https://www.npmjs.com/package/ts-node), which will be used to execute the sample code provided in JuneoJS).
{% endhint %}

### Create a Supernet

The first step is to transfer the last juneogo binary, _srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e,_ to the server running your node as it will be required to later deploy a VM on your Supernet.

Please stop your node and transfer the binary to the `~/.juneogo/plugins` directory.&#x20;

Then, allow execution permissions for this binary, and run your node again:

```bash
chmod +x .juneogo/plugins/srEr2XGGtowDVNQ6YgXcdUb16FGknssLTGUFYg7iMqESJ4h8e

./juneogo
```

Next, please download the [juneojs-examples](https://github.com/Juneo-io/juneojs-examples) repository on your local machine. If you have [git](https://git-scm.com/) installed on your system, please execute the following in your command line:

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

To perform the transactions required to deploy a Supernet, you will need to have funds on the P-chain.  In this tutorial, we will first be crossing assets from the JUNE-chain to the JVM-chain, and then from  the JVM-chain to the P-chain.

In a command line window open in the root of the JuneoJS library, execute the following:

```bash
npx ts-node ./src/docs/crossJUNEtoJVM.ts
```

This will cross 1.1 JUNE from the JUNE-chain to the JVM-chain. After this, we will cross 1 JUNE from the JVM-chain to the P-chain by executing the following command:

```bash
npx ts-node ./src/docs/crossJVMtoP.ts
```

The next step is the Supernet creation. Please execute the following in the comand line:

```
npx ts-node ./src/supernet/createSupernet.ts
```

This will produce an output to the terminal containing your supernetID. Example: _ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwh._&#x20;

This is the id of the transaction that has created your Supernet, and is the id of the Supernet as well. Please save it as we will needed in the following steps.

### Add a validator to the Supernet

The next step is to perform the addSupernetValidator transaction.&#x20;

{% hint style="info" %}
If your Supernet has no validators, all blockchains in that Supernet will be inactive and will not be able to process transactions.
{% endhint %}

Please open the file `./src/supernet/addSupernetValidator.ts` in your code editor, and update the following variables to contain the correct values. Example:

```typescript
const nodeId: string = 'NodeID-B2GHMQ8GF6FyrvmPUX6miaGeuVLH9UwHr'
const supernetId: string = 'ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP'
const durationInDays: number = 4 // number of days you will validate your Supernet
```

We will keep the **durationInDays** variable the same. However, you may update it to a diffent value.&#x20;

{% hint style="info" %}
The variable **durationInDays** must be less than the amount of remaining days your node will be validating the Primary network.

If your node has 5 days (or a little over 4 days) left of Primary Network validation, you must set **durationInDays** to 4 or less.
{% endhint %}

{% hint style="info" %}
If **your node** is the only node validating your Supernet - once your node stops validating it, all chains within it will not be able to process transactions.

You would have to re-perform the **Add Supernet Validator** transaction for your Supernet's chains to process transactions again.
{% endhint %}

Then, execute this file:

```sh
npx ts-node ./src/supernet/addSupernetValidator.ts
```

This will add our node as a validator for our supernet. However, we need to perform an additional step before our node can begin validating our Supernet.&#x20;

Please open the command line on the server running your node, stop the node, and create a file titled `config.json` in the home directory.&#x20;



```
cd ~

nano config.json
```

In it, please specify the Supernets you want your node to track (in this case, the Supernet you have just created). Example:

```json
{
 "track-supernets":"ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP"
}
```

After saving the file, you may run your node again with the following command:

```bash
./juneogo --config-file="./config.json"
```

Your node is now tracking the Supernet you have just created. From here, we may proceed to the next step: Deploy a VM.
