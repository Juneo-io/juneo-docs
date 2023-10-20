# Node Backup and Restore

It is important to be prepared in the event that your node suffers a failure due to hardware and software issues by having a backup.

However, a node can accumulate a database many Gigabytes in size which can be expensive and time-consuming to backup and restore. It is only essential to back up those files which are uniquely generated for our node. In the case of a Juneo node, these files are files that identify our node in the network (that define our nodeID).

The validation and delegation transactions are stored on the blockchain, so it is not necessary to back these up as they will be restored during the node bootstrapping process.

### Node ID

{% hint style="danger" %}
If there are more than one nodes running on the network which have the same Node ID, other nodes will randomly communicate with one of these nodes. If this NodeID is that of a validator node, its uptime calculation will be negatively effected which may result in loss of staked rewards. Please make sure that only one node is running with your NodeID.
{% endhint %}

The NodeID is unique identifier used to differentiate your node from other nodes in the Juneo network. It is a string formatted such as `NodeID-5mb46qkSBj81k9g9e4VFjGGSbaaSLFRzD`.

The NodeID is identified by two files:

* `staker.crt`
* `staker.key`

You will be able to find these files in the juneogo installation, specifically in `~/.juneogo/staking`.

This folder contains a third file:

* `signer.key`

In order to backup our node, all we need to do is bootstrap a new one which contains these three files in the staker directory. If we were to remove these files and restart our node, a new version of these files would be created and our node would be assigned a new nodeID.

### Backup

To backup our node, we need to store the files `signer.key`, `staker.crt` and `staker.key`. It is advised that you store these files in a safe and private location, potentially multiple different locations (private USB stick or cloud storage, personal computer and similiar). Storing them on different locations decreases the risk of losing your node and any potential staking rewards.

{% hint style="warning" %}
If someone gets access of your staker files, they will not be able to withdraw any staking funds as they are controlled by your wallet private key. However, they will be able to re-create your node somewhere else which will negatively affect the uptime calculation of your validator node, potentially resulting in lost staking rewards.

Make sure that your staker files are private and secure.
{% endhint %}

#### From Local Node

If you're running the node locally, all you have to do is navigate to where these files are stored, and store them in a safe location.

You will find these files in the `~/.juneogo/staking` directory. Simply select and copy the files to the backup location. You don't need to stop the node in order to do this.

#### From Remote Node Using `scp`

`scp` is a 'secure copy' command line program, available built-in on Linux and MacOS computers. There is also a Windows version, `pscp`, as part of the [PuTTY](https://www.chiark.greenend.org.uk/\~sgtatham/putty/latest.html) package. If using `pscp`, in the following commands replace each usage of `scp` with `pscp -scp`.

When you have means of remote login into the machine, you can copy the files over with the following command:

```bash
scp -r ubuntu@PUBLICIP:/home/ubuntu/.juneogo/staking ~/juneo_backup
```

This assumes the username on the machine is `ubuntu`, replace with the correct username in both places if it is different. Also, replace `PUBLICIP` with the actual public IP of the machine. If `scp` doesn't automatically use your downloaded SSH key, you can point to it manually:

```bash
scp -i /path/to/the/key.pem -r ubuntu@PUBLICIP:/home/ubuntu/.juneogo/staking ~/juneo_backup
```

Once executed, this command will create  `juneo_backup` directory in you home directory and place the staker files in it. You need to store them somewhere safe.

### Restore

In order to restore a node from a backup, we need to do the reverse: we need to place `staker.key`, `staker.crt` and `signer.key` we previously backed up to the working direcotry of our node.

First we need to [run a Juneo node](../build/set-up-and-connect-a-node.md), which will create a new NodeID, which we will replace. After the node has successfully installed, we should stop it.

#### To Local Node

If you are running your node locally, copy the `staker.key`, `staker.crt` and `signer.key` files from the backup location into the working directory of the node, which will be found in  `~/.juneogo/staking`.

#### To Remote Node Using `scp`

This will be a reverse process from backing up the staking files on a remote node - we now need to copy the `staker.key`, `staker.crt` and `signer.key` files from the backup location into the remote working directory. If we assume that the backup files are in the same location specified in the **`scp`** backup above:

```bash
scp ~/juneo_backup/staker.* ubuntu@PUBLICIP:/home/ubuntu/.juneogo/staking
```

or if we need to specify the location of the SSH key:

```bash
scp -i /path/to/the/key.pem ~/juneo_backup/staker.* ubuntu@PUBLICIP:/home/ubuntu/.juneogo/staking
```

Please replace `ubuntu` with the correct username if it is different, and replace `PUBLICIP` with the actual public IP of your remote server, as well as the path to the SSH key if used.

#### Restart Node and Verify

After replacing the staker files, you may start the node.

You can verify that the update has been completed by executing the following command:

```json
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

You should see your original NodeID, meaning that your node has successfully been restored.

