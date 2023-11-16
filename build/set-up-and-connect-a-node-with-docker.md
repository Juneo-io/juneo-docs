# Set Up and Connect a node with Docker

How to set up and connect a node to the Socotra Testnet v1 network using docker.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requierments](node-requirements.md).
{% endhint %}

## Setup and Connect a node to the Socotra Testnet v1[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

### 1. Docker and docker-compose <a href="#user-content-1-docker-and-docker-compose" id="user-content-1-docker-and-docker-compose"></a>

Please make sure that you have Docker and docker-compose installed on your system. For more information, please refer to their documentation: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

### 2. Configure the node <a href="#user-content-2-configure-the-node" id="user-content-2-configure-the-node"></a>

Copy the files to the server in your home directory with the following command:

```
git clone https://github.com/Juneo-io/juneogo-docker
```

By default, your node will not accept remote RPC calls. If you would like to enable remote calls to your node, please expose port `9650` in the `docker-compose.yml` file:

```yaml
ports:
  - 9650:9650 # port for API calls - will enable remote RPC calls to your node (mandatory for Supernet/ Blockchain deployers)
```

### 3. Run JuneoGo <a href="#user-content-3-run-juneogo" id="user-content-3-run-juneogo"></a>

You may run JuneoGo using http or https.

#### 3.1) Run JuneoGo with HTTP <a href="#user-content-31-run-juneogo-with-http" id="user-content-31-run-juneogo-with-http"></a>

To run your node with http, please open the juneogo-docker directory in your command line and execute:

```bash
docker-compose build

docker-compose up -d juneogo
```

This will start bootstrapping your node.

#### 3.2) Run JuneoGo with HTTPS <a href="#user-content-32-run-juneogo-with-https" id="user-content-32-run-juneogo-with-https"></a>

To run your node with https, please set up your custom domain to point to your machine's public ip address.

Next, please update the file Caddyfile located in `juneogo-docker/caddy` to contain your domain instead of the sample url.

Example:

```yaml
juneo.node.com {
    reverse_proxy juneogo:9650
}
```

After this, please execute:

```bash
docker-compose build

docker-compose up -d
```

This will start bootstrapping your node.

### 4. Boostrapping status <a href="#user-content-4-boostrapping-status" id="user-content-4-boostrapping-status"></a>

You can check your node's bootstrapping status with the following RPC call:

```bash
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"JUNE"
    }
}' -H 'content-type:application/json;' 192.168.10.2:9650/ext/info
```

Example response:

```bash
{
  "jsonrpc": "2.0",
  "result": {
    "isBootstrapped": true
  },
  "id": 1
}
```

Once your node has fully boostrapped, navigate to `juneogo-docker/juneogo/` and execute the command in the following format:

```
sudo chown -R [your_user_name] .juneogo/
```

Example (if the user is `juneogo`):

```bash
sudo chown -R juneogo .juneogo/
```

To enter the juneogo Docker container, please execute:

```bash
docker exec -ti juneogo /bin/bash
```
