# Sentry Nodes

### Setting up a Validator <a href="#setting-up-a-validator" id="setting-up-a-validator"></a>

When setting up a validator there are countless ways to configure your setup. This guide is aimed at showing one of them, the sentry node design. This design is mainly for DDOS prevention.

![](<../.gitbook/assets/Sentry nodes picture (2).png>)

The diagram is based on AWS, other cloud providers will have similar solutions to design a solution. Running nodes is not limited to cloud providers, you can run nodes on bare metal systems as well. The architecture will be the same no matter which setup you decide to go with.

The proposed network diagram is similar to the classical backend/frontend separation of services in a corporate environment. The “backend” in this case is the private network of the validator in the data center. The data center network might involve multiple subnets, firewalls and redundancy devices, which is not detailed on this diagram. The important point is that the data center allows direct connectivity to the chosen cloud environment. Amazon AWS has “Direct Connect”, while Google Cloud has “Partner Interconnect”. This is a dedicated connection to the cloud provider (usually directly to your virtual private cloud instance in one of the regions).

All sentry nodes (the “frontend”) connect to the validator using this private connection. The validator does not have a public IP address to provide its services.

Amazon has multiple availability zones within a region. One can install sentry nodes in other regions too. In this case the second, third and further regions need to have a private connection to the validator node. This can be achieved by VPC Peering (“VPC Network Peering” in Google Cloud). In this case, the second, third and further region sentry nodes will be directed to the first region and through the direct connect to the data center, arriving to the validator.

A more persistent solution (not detailed on the diagram) is to have multiple direct connections to different regions from the data center. This way VPC Peering is not mandatory, although still beneficial for the sentry nodes. This overcomes the risk of depending on one region. It is more costly.

![](../.gitbook/assets/image.png)

The validator will only talk to the sentry that are provided, the sentry nodes will communicate to the validator via a secret connection and the rest of the network through a normal connection. The sentry nodes do have the option of communicating with each other as well.

### Technical Setup

{% hint style="info" %}
For more information on technical set up, you may refer [here](https://docs.tendermint.com/master/nodes/validators.html).
{% endhint %}

The main aim of a sentry node is to protect our validator nodes from a huge number of queries thus overloading the port (DDOS).

**Characteristics of a Sentry Node**

* The sentry node has to be a full-node
* It has to be on a different server from the one you have your validator node on
* It does not have to be in the same locale as your validator node
* Having multiple sentry nodes would have even greater protection against

**How you should configure your config.toml file for your VALIDATOR NODE**

{% hint style="info" %}
you may access your config.toml file in .fxcore/config/config.toml
{% endhint %}

* `pex:` boolean. This turns the peer exchange reactor on or off for a node. When `pex=false`, only the `persistent-peers` list is available for connection. **This should be set to `pex=false`**.
* `seed_mode`: boolean. The main function of the seed\_mode is to provide more node addresses to the network. It will record all the node addresses that have been connected to it, and as long as you connect to it, it will tell you all the node information it records. This way you can connect to a node quickly. The seed node will disconnect from you immediately after giving you all the node information, so it is not recommended that the validator node enable the seed mode.  **This should be set to `seed_mode=false`**.
* `addr_book_strict:` boolean. By default nodes with a routable address will be considered for connection. If this setting is turned off (false), non-routable IP addresses, like addresses in a private network can be added to the address book. **This should be set to `addr_book_strict=false`**.
