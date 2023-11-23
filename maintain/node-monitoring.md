# Node Monitoring

If you wish to monitor your JuneoGo node, it is possible to do so by using the [juneogo-monitoring](https://github.com/Juneo-io/juneogo-monitoring) repository. This repository implements [Prometheus ](https://prometheus.io/)and [Graphana ](https://grafana.com/)to monitor, store, and display key JuneoGo node metrics.

## Installation instructions

{% hint style="info" %}
These installation instructions assume that you already have Docker and docker-compose installed on your system. For more information, please refer to their documentation: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
{% endhint %}

First, open the command line on the machine running your node and execute the following command:

```bash
git clone https://github.com/Juneo-io/juneogo-monitoring
```

### Configuring Prometheus

{% hint style="info" %}
If you are running juneogo using [juneogo-docker](https://github.com/Juneo-io/juneogo-docker), you may skip this configuration step and proceed to the [Start Monitoring](node-monitoring.md#start-monitoring) step.
{% endhint %}

{% hint style="warning" %}
Users who set up their JuneoGo node [manually ](../build/set-up-and-connect-a-node-manually.md)or with the [install script](../build/set-up-and-connect-a-node.md) - it is important to follow this configuration step for all metrics to be monitored.
{% endhint %}

If you are publicly exposing port `9650` with the juneogo `http-host` configuration flag set to your public ip address (allowing remote RPC calls to your node), you will need to update the `./prometheus/prometheus.yml` file.

Specifically, you will have to update the `"targets"` value for the job\_name of `"juneogo"` to your public ip address rather than localhost.

Example:

```yaml
- job_name: "juneogo"
    static_configs:
      - targets: ['XX.XX.XX.XX:9650']
```

If you are not publicly exposting port `9650`, you may skip this step.

### Start Monitoring

In order to start your monitoring your node, please execute the following command:

```bash
docker-compose up -d
```

Grafana should be accessible through port 3000, use login: **admin** and password: **admin.**

Prometheus should be accessible through port 9090.
