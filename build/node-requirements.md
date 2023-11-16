# Node Requirements

The hardware and software requirements for running a Juneo node.

### Requirements

#### Computer Hardware and OS <a href="#computer-hardware-and-os" id="computer-hardware-and-os"></a>

Juneogo is a lightweight protocol. Note that as network usage increases, hardware requirements may change.

* CPU: 4 cores
* RAM: 8 GiB
* Storage: 160 GiB SSD (500 GiB SSD for Mainnet)
* OS: Ubuntu 20.04/ 22.04

#### Networking[​](https://docs.avax.network/nodes/build/run-avalanche-node-manually#networking) <a href="#networking" id="networking"></a>

To run successfully, juneogo needs to accept connections from the Internet on the network port `9651` (and optionally port `9650` to allow remote RPC calls to your node). Before you proceed with the installation, you need to determine the networking environment your node will run in.

#### **Running on a Cloud Provider**[**​**](https://docs.avax.network/nodes/build/run-avalanche-node-manually#running-on-a-cloud-provider)

If your node is running on a cloud provider computer instance, it will have a static IP. Find out what that static IP is, or set it up if you didn't already.

#### **Running on a Home Connection**[**​**](https://docs.avax.network/nodes/build/run-avalanche-node-manually#running-on-a-home-connection)

If you're running a node on a computer that is on a residential internet connection, you have a dynamic IP (meaning your IP will change periodically). You will need to set up inbound port forwarding of port `9651` from the internet to the computer the node is installed on.