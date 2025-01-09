# Set Up and Connect a node with Docker

How to set up and connect a node to the Mainnet using docker.

{% hint style="warning" %}
Before proceeding, please make sure that your machine meets the hardware and software [node requirements](node-requirements.md).
{% endhint %}

## Setup and Connect a node to the Mainnet[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#run-an-avalanche-node) <a href="#run-an-avalanche-node" id="run-an-avalanche-node"></a>

### 1. Docker and docker-compose <a href="#user-content-1-docker-and-docker-compose" id="user-content-1-docker-and-docker-compose"></a>

Please make sure that you have Docker and docker-compose installed on your system. For more information, please refer to their documentation: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

### 2. Configure the node <a href="#user-content-2-configure-the-node" id="user-content-2-configure-the-node"></a>

Copy the files to the server in your home directory with the following command:

```
git clone https://github.com/Juneo-io/juneogo-docker
```

By default, your node will not accept remote RPC calls. If you would like to enable remote calls to your node, please expose port `9650` in the `docker-compose.yml` file:

{% hint style="warning" %}
This configuration is only needed if you don't use HTTPS
{% endhint %}

```yaml
ports:
  - 9650:9650 # port for API calls - will enable remote RPC calls to your node (mandatory for Supernet/ Blockchain deployers)
```

{% hint style="warning" %}
If you want to connect the node to the Socotra testnet, you need to update the `config.json` file with the `network-id` flag :&#x20;

```bash
{
  "network-id":"socotra"
}
```
{% endhint %}

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

Before starting Caddy, ensure you have set up your `.env` file with the necessary Caddy environment variables:

```
CADDY_EMAIL=your-email@example.com
CADDY_DOMAIN=your-domain.com
CADDY_USER=your-username
CADDY_PASSWORD=your-password
```

To securely export metrics, Caddy is configured to handle metrics endpoints. Metrics are securely exposed behind Caddy. If you want to have an API node, you need to expose `juneogo:9650` without credentials in the Caddyfile.\
\
Here is the current config :&#x20;

```yaml
{
  email {$CADDY_EMAIL}
}

{$CADDY_DOMAIN} {
    
    handle /ext/metrics* {
        reverse_proxy juneogo:9650
        basic_auth {
            {$CADDY_USER} {$CADDY_PASSWORD}
        }
    }

    handle /metrics* {
        reverse_proxy node_exporter:9100
        basic_auth {
            {$CADDY_USER} {$CADDY_PASSWORD}
        }
    }

    handle {
        respond "Access Denied" 403
    }
}


```

You need to update the config by removing the basic authentification and/or by adding theses lines :

```yaml
{$CADDY_DOMAIN} {
    reverse_proxy juneogo:9650
}
```

After this, please open the juneogo-docker directory in your command line and execute:

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

If necessary, you can enter the juneogo Docker container with the following command:

```bash
docker exec -ti juneogo /bin/bash
```
