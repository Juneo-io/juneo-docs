---
description: How to add your JUNEO node as a validator to the Primary network.
---

# Add a Node to the Validator Set

After successfully bootstrapping your node, you may add it as a validator on the primary network (after which you may create Supernets and deploy blockchains on them).

### Stakes and fees

* The minimal stake on Mainnet is 50 JUNE, where as the maximum stake is 45,000 JUNE. On the Socotra network, the minimal stake is 1 JUNE, and the maximal stake is 1,000,000 JUNE. You will also need additional funds to pay for transaction fees.
* The minimal staking period is 14 days on Mainnet and 1 day on the Socotra network. The maximal staking period for both networks is 365 days.
* Any delegator staking their funds on your node will pay you the delegation fee, which is 12% of their reward. Delegators can stake no more than x4 of your own stake.

{% hint style="info" %}
A node cannot be a validator on a Supernet longer than it is a validator on the primary supernet. If you wish to deploy a supernet and add your node as a validator, we recommend staking on the primary supernet for at least 15 days on Mainnet, and at least 2 days on Socotra. Once you have staked your tokens, you will be able to deploy a Supernet and add your node as a validator to it for the minimum required validation period.
{% endhint %}

|                        | Mainnet     | Socotra         |
| ---------------------- | ----------- | --------------- |
| Minimal stake          | 50 JUNE     | 1 JUNE          |
| Maximal stake          | 45,000 JUNE |  1,000,000 JUNE |
| Minimal staking period | 14 days     | 1 day           |
| Maximal staking period | 365 days    | 365 days        |

### Node requirements

| OS      | Ubuntu 20.04/ 22.04                  |
| ------- | ------------------------------------ |
| CPU     | 4 cores                              |
| RAM     | 8 GiB                                |
| Storage | 160 GiB SSD (500 GiB SSD on Mainnet) |
| Network | 5 Mbps                               |

### Network

At least 5 Mbps bandwidth (both upload and download) is required to run a validator node.

The node needs to accept connections from the Internet on the network ports `9650` and `9651`. To enable ports access on macOS, use Apple [documentation](https://support.apple.com/guide/mac-help/change-firewall-settings-on-mac-mh11783/mac). On Linux, use `ufw` firewall:

```bash
sudo ufw allow 9650
sudo ufw allow 9651
```

{% hint style="warning" %}
Running your node behind NAT is not recommended. Home installation is not recommended except for testing purposes.
{% endhint %}

If you're running a node on a home computer, it's most likely that you have a dynamic IP. You will need to either ask your ISP to assign a permanent public address, or set up port forwarding of ports `9650` and `9651` from the internet to your node computer. Please reference your router documentation.

An active node maintains thousands of live TCP connections, keep this in mind when choosing the router for such installation. A cheap router can cause network problems and make your node considered offline because of poor connection. Being online less than 80% of staking period means losing your rewards.

### Date and time synchronization

{% hint style="danger" %}
Having your node's time correctly synchronized is extremely important. Incorrect time synchronization may cause your node to be marked as offline resulting in lost staking rewards.
{% endhint %}

To enable time synchronization on Linux, please run the the command `timedatectl` in your terminal.&#x20;

If the **NTP service** parameter is set to **no**, then your node't time may get out of sync. Set up time synchronization by running the command:

```bash
timedatectl set-ntp on
```

Run `timedatectl` again, and the parameter **System clock synchronized** should now be set to **yes.**

For time synchronization on macOS, please follow their [documentation](https://support.apple.com/guide/mac-help/set-the-date-and-time-automatically-mchlp2996/mac).



### Adding a validator using the graphic interface

The first step to validate the Socotra network is to retrieve our Node ID. Open the terminal on the server running your node, and execute the following call:

```bash
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json' 127.0.0.1:9650/ext/info
```

Example response:

```bash
{
  "jsonrpc": "2.0",
  "result": {
    "nodeID": "NodeID-HkHfVnfbpiLKDBrW7wiy8HET456UKnr6C"
  },
  "id": 1
}
```

Next, copy the value for the nodeID parameter and log into [mcnwallet.io](https://www.mcnwallet.io/).

Please make sure that you have a balance of at least 1 JUNE in the P-chain, and additional JUNE for transaction costs. For this purpose, you may perform Cross-chain transactions to transfer JUNE from any of the other two chains to the P-chain in the **Cross-chain** page.

In the mcnwallet, navigate to the **Stake** page, and click on the **Validate** card. We shall enter our nodeID, set the staking amount to 1 JUNE, and the validation period to 15 days.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Next, click Validate to add your node to the Validator set.&#x20;

\
After this transaction has been completed, you may navigate to [https://mcnscan.io/validator/validator-list](https://mcnscan.io/validator/validator-list) and enter your nodeID in the search bar, which will show you the status of your node:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
