---
description: >-
  Ensure node reliability by backing up essential files like nodeID for quick
  recovery
---

# Node Backup and Restore

It is important to be prepared in the event that your node suffers a failure due to hardware and software issues by having a backup.

However, a node can accumulate a database many Gigabytes in size which can be expensive and time-consuming to backup and restore (but is necessary if you require access to historical data on your node). For most users, it is only essential to back up those files which are uniquely generated for our node. In the case of a Juneo Supernet node, these files are files that identify our node in the network (that define our nodeID).

The validation and delegation transactions are stored on the blockchain, so it is not necessary to back these up as they will be restored during the node bootstrapping process.

## Node ID

{% hint style="danger" %}
If there are more than one nodes running on the network which have the same Node ID, other nodes will randomly communicate with one of these nodes. If this NodeID is that of a validator node, its uptime calculation will be negatively effected which may result in loss of staked rewards. Please make sure that only one node is running with your NodeID.
{% endhint %}

The NodeID is a unique identifier used to differentiate your node from other nodes in the Juneo Supernet network. It is a string formatted such as `NodeID-5mb46qkSBj81k9g9e4VFjGGSbaaSLFRzD`.

The NodeID is identified by two files:

* `staker.crt`
* `staker.key`

You will be able to find these files in the juneogo installation, specifically in `~/.juneogo/staking`.

This folder contains a third file:

* `signer.key`

In order to backup our node, all we need to do is bootstrap a new one which contains these three files in the `staking` directory. If we were to remove these files and restart our node, a new version of these files would be created and our node would be assigned a new nodeID.

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

`scp` is a 'secure copy' command line program, available built-in on Linux and MacOS computers. There is also a Windows version, `pscp`, as part of the [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) package. If using `pscp`, in the following commands replace each usage of `scp` with `pscp -scp`.

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

First we need to [run a Juneo Supernet node](broken-reference), which will create a new NodeID, which we will replace. After the node has successfully installed, we should stop it.

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

## Database

Certain situations may require you to bootstrap a node using an existing database (such as preserving historical data, or decreasing node sync time). Such situations require that you have a backup of a database you with to restore.

### Database Backup

{% hint style="warning" %}
Before continuing, please make sure that your node is not running while you are backing up your database, otherwise you risk corrupting the data.
{% endhint %}

After your node is no longer running, the next step is to zip the database directory, which you may do with the following command:

```bash
zip -r juneogo_db_backup.zip .juneogo/db
```

Please note, this process may take > 30 minutes. Once it is complete, you may start your node again.

{% hint style="info" %}
When backing up your database, you should copy the configuration files to have the correct pruning parameters for all chains. You may find these in the `.juneogo/configs/chains` directory.
{% endhint %}

Next, please transfer this database file to another machine where you wish to store the backup. This can be done using **`scp`**:

```bash
scp -r ubuntu@PUBLICIP:/home/ubuntu/juneogo_db_backup.zip ~/juneogo_db_backup.zip
```

If `scp` doesn't automatically use your downloaded SSH key, you can point to it manually:

```
scp -i /path/to/the/key.pem -r ubuntu@PUBLICIP:/home/ubuntu/juneogo_db_backup.zip ~/juneogo_db_backup.zip
```

Please replace `ubuntu` with the correct username if it is different, and replace `PUBLICIP` with the actual public IP of your remote server, as well as the path to the SSH key if used.

After this, you should have a `juneogo_db_backup.zip` directory in your home directory.

### Database Restore

This step assumes that you have a database backup at `~/juneogo_db_backup.zip`.

Once you have a node that is installed correctly and running, open the command like on the machine and stop your node.

{% hint style="warning" %}
Before continuing, please make sure that your node is not running while you are restoring your database, otherwise you risk corrupting the data.
{% endhint %}

The next step is to transfer your database backup to your node in the `.juneogo` directory.&#x20;

{% hint style="info" %}
Please make sure to transfer previously backed up configuration files for your chains, so that your node can run with the correct initial pruning parameters.
{% endhint %}

Next, we will rename the current database directory on your node (which you may remove later if the database restoration is successful):

```bash
mv .juneogo/db .juneogo/db_old
```

Next, please unzip the database backup folder:

```bash
unzip .juneogo/juneogo_db_backup.zip
```

You should now have the `db` directory in  the `.juneogo` directory. After this, you may re-start your node.&#x20;

Your node will start catching up with blocks it missed while it was offline. Once you have verified that your node is fully bootstraped and running properly, you may remove the zip file.

```bash
rm .juneogo/juneogo_db_backup.zip
```
