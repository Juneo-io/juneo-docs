# Deploy a VM

After you have [created your Supernet](create-a-supernet.md), you can deploy a VM on it.

{% hint style="info" %}
The pre-requisite for this tutorial is that you have already [added a JUNEO validator node](../validate/add-a-validator.md), [created a Supernet](create-a-supernet.md) and [added a Supernet validator](create-a-supernet.md#add-supernet-validator).
{% endhint %}

We will be using the example files found in the repository [juneogo-examples](https://github.com/Juneo-io/juneojs-examples) to deploy an supernetevm on our existing Supernet.

{% hint style="info" %}
Please make sure that you have [Node.JS](https://nodejs.org/en) installed on your system, along with the npm package[ ts-node](https://www.npmjs.com/package/ts-node) (which will be used to execute the sample code provided in JuneoJS).
{% endhint %}

### Deploying a VM

Assuming you have followed the [steps for creating a supernet](create-a-supernet.md), you should have the [juneojs-examples](https://github.com/Juneo-io/juneojs-examples) folder available. Open [juneojs-examples](https://github.com/Juneo-io/juneojs-examples) directory in a code editor of your choice, create a .env file from the provided .env.example, and paste your mnemonic phrase in the MNEMONIC variable. Example:

```sh
MNEMONIC="raven whip pave toy benefit moment twin acid wasp satisfy crash april"
```

Next, open the file `./src/supernet/createChain.ts`. Please find and update the following variables to contain the correct values. Example:

```typescript
const supernetId: string = 'ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP' // your supernet id

const chainName: string = 'Chain A'

const chainId: number = 330333

const genesisMintAddress: string = '0x44542FD7C3F096aE54Cc07833b1C0Dcf68B7790C' // your wallet address here
```

{% hint style="danger" %}
Never use the same **chainId** value more than once- this can cause unexpected errors for your Supernet chains in the future.
{% endhint %}

{% hint style="info" %}
Please make sure to specify the `chainId` variable to a value that is not already taken by another chain.

For **Socotra v1**, please use a random number from 300,000 - 399,999.\
For **Socotra v2**, please use a random number from 200,000 - 299,999.

For **Mainnet,** please use a number starting from 450,000.
{% endhint %}

After this, execute this file in the command line using ts-node:

```bash
npx ts-node ./src/supernet/createChain.ts
```

Your command line output should resemble the following, indicating that your VM was successfully deployed:

<pre><code><strong>Created chain with id: 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D
</strong></code></pre>

### Indexation of your chain on the explorer

For your chain to be eligible for indexation on the explorer, you will need to disable pruning for your newly created chain, allowing archive node functionality. This is done by creating a `config.json` file on your JUNEO validator node in the location `~/.juneogo/configs/chains/chainId.`

For a chain which has the id of `2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D`, you would execute the following commands from the home directory:

```
cd .juneogo

mkdir configs
cd configs

mkdir chains
cd chains

mkdir 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D
cd 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D

nano config.json
```

Next, paste the following inside the `config.json` file:

```json
{
"pruning-enabled": false,
  "eth-apis": ["public-eth", "public-eth-filter","net","web3","internal-public-eth","internal-public-blockchain","internal-public-transaction-pool","internal-public-debug","debug-tracer"]
}
```



In addition to this, it is required that your node accepts API calls from remote machines. For this, we have to make updates to the `config.json` file found in the home directory:

```bash
cd ~
nano config.json
```

This file should be updated to include the config flag `http-host`, and its value should be set to your public IP address. Example:

```json
"http-host":"XXX:XXX:XXX:XXX"
```

Following the previous steps from the documentation, this `config.json` file should resemble the following format:

```json
{
 "track-supernets":"ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP",
 "http-host":"XXX.XXX.XXX.XXX"
}
```

After updating this file, please re-start your node from the home directory:

```bash
./juneogo --config-file="./config.json"
```



After this, the next step is to submit your Supernet and Blockchain data to us for indexation.

{% hint style="info" %}
Before proceeding with the next step, please perform a few transactions on your chain using MetaMask, such as sending some tokens to any address.
{% endhint %}

Please submit your Supernet and Blockchain information to us in the following format:

<pre><code><strong>Supernet
</strong><strong>- Name
</strong>- Description
- id
<strong>
</strong><strong>Blockchain
</strong><strong>- Name 
</strong>- Description 
- Currency (in ALL-CAPS format)
- Decimals 
- Host (http://xxx.xxx.xxx.xxx:9650 or with domain name) 
- rpc (http://xxx.xxx.xxx.xxx:9650/ext/bc/id/rpc)
- Logo (min 273x273 or greater.
        In .png format. 
        The width:height ratio should be 1:1)
- id
</code></pre>

{% hint style="info" %}
Please use unique descriptive names for your Supernet and Blockchain. Avoid names such as "Test Supernet" or "Test Blockchain".
{% endhint %}

Example:

<pre><code>Supernet
- Name: Supernet ABC
- Description: This is a supernet created by
- id: ZxTjijy4iNthRzuFFzMH5RS2BgJemYxwgZbzqzEhZJWqSnwhP

Blockchain
- Name: Chain ABC
- Description: This is a Blockchain developed
<strong>- Currency: ABC22
</strong>- Decimals: 18
- Host: http://XXX.XXX.XXX.XXX:9650 
- rpc: http://XXX.XXX.XXX.XXX:9650/ext/bc/2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D/rpc
- Logo: [your_logo_goes_here]
- id: 2LxhUB7z7yxvBkHiiwp8MrALgy7rka3y6MriL2JAeX858wUK5D
</code></pre>
