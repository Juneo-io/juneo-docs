# Node Informations

## How to get your node informations ?

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
    "nodeID": "NodeID-4JfgcoMWBpxCQL5VmyQ1f6L36mUbLLBga",
    "nodePOP": {
      "publicKey": "MyBLSKey",
      "proofOfPossession": "MyBLSSignature",
    }
  },
  "id": 1
}
```

Next, copy the value for the `nodeID`, `publicKey`, and `proofOfPossession` from the response.



