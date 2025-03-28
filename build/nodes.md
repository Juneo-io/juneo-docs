# Advanced Node Configuration

## Archive Node

**Archive Node** is a full node that validates transactions, maintains the blockchain state, and stores the entire historical record from the genesis block onward. Unlike standard full nodes, it retains all state changes, making it essential for explorers, analytics, and data indexing. \
\
An Archive Node is configured to disable database pruning and retain all historical states of the chains it validates. While storage-intensive, it ensures unparalleled access to blockchain history for data retrieval, auditing, and analysis.

### Chain Configuration

You can then configure each chain individually or only the ones you wish to use. Each chain must have its own **config.json** file, located in the `config` directory.

```bash
BCH1/  DAI1/  DOGE1/  EUR1/  GLD1/  JUNE/  LINK1/  LTC1/  mBTC1/  SGD1/  USD1/  USDT1/
```

For example, here is the configuration file for the **JUNE** chain:

```json
{
  "state-sync-enabled": false,
  "pruning-enabled": false
}
```

The `"pruning-enabled": false` parameter ensures that pruning is disabled, allowing the node to operate in Archive mode by retaining all historical state data for the chain.

## Full Node

A **Full Node** is configured with database pruning enabled for the chains it validates, retaining only the most recent state of those chains. It stores the current blockchain state and the most recent blocks while validating new blocks.&#x20;

Full nodes process transactions, execute smart contracts, and serve blockchain data. While they can access some historical data through tracing, they are not ideal for deep historical queries. This approach reduces disk space usage and improves performance, though full nodes do not store all historical states like Archive Nodes.

### Chain Configuration

You can then configure each chain individually or only the ones you wish to use. Each chain must have its own **config.json** file, located in the `config` directory.

```bash
BCH1/  DAI1/  DOGE1/  EUR1/  GLD1/  JUNE/  LINK1/  LTC1/  mBTC1/  SGD1/  USD1/  USDT1/
```

For instance, here is the configuration file for the **JUNE** chain tailored for a Full Node:

```json
{
  "state-sync-enabled": false
}
```

