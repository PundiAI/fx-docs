# Sentry Nodes

### Setting up a Validator <a href="#setting-up-a-validator" id="setting-up-a-validator"></a>

When setting up a validator there are countless ways to configure your setup. This guide is aimed at showing one of them, the sentry node design. This design is mainly for DDOS prevention.

![](<../.gitbook/assets/Sentry nodes picture (2) (1).png>)

The diagram is based on AWS, other cloud providers will have similar solutions to design a solution. Running nodes is not limited to cloud providers, you can run nodes on bare metal systems as well. The architecture will be the same no matter which setup you decide to go with.

The proposed network diagram is similar to the classical backend/frontend separation of services in a corporate environment. The “backend” in this case is the private network of the validator in the data center. The data center network might involve multiple subnets, firewalls and redundancy devices, which is not detailed on this diagram. The important point is that the data center allows direct connectivity to the chosen cloud environment. Amazon AWS has “Direct Connect”, while Google Cloud has “Partner Interconnect”. This is a dedicated connection to the cloud provider (usually directly to your virtual private cloud instance in one of the regions).

All sentry nodes (the “frontend”) connect to the validator using this private connection. The validator does not have a public IP address to provide its services.

Amazon has multiple availability zones within a region. One can install sentry nodes in other regions too. In this case the second, third and further regions need to have a private connection to the validator node. This can be achieved by VPC Peering (“VPC Network Peering” in Google Cloud). In this case, the second, third and further region sentry nodes will be directed to the first region and through the direct connect to the data center, arriving to the validator.

A more persistent solution (not detailed on the diagram) is to have multiple direct connections to different regions from the data center. This way VPC Peering is not mandatory, although still beneficial for the sentry nodes. This overcomes the risk of depending on one region. It is more costly.

![](<../.gitbook/assets/image (11) (1).png>)

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

**How you should configure your config.toml file for your **<mark style="color:blue;">**VALIDATOR NODE**</mark>**/**<mark style="color:red;">**SENTRY NODE**</mark>

{% hint style="info" %}
you may access your config.toml file in .fxcore/config/config.toml
{% endhint %}

* `pex:` boolean. This turns the peer exchange reactor on or off for a node. When `pex=false`, only the `persistent-peers` list is available for connection. <mark style="color:blue;">**This should be set to**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**`pex=false`**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**so it does not gossip to the entire network**</mark><mark style="color:blue;">.</mark> <mark style="color:red;">**The sentry nodes should be able to talk to the entire network hence why**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`pex=true`**</mark><mark style="color:red;">**.**</mark>
* `seed_mode`: boolean. The main function of the seed\_mode is to provide more node addresses to the network. It will record all the node addresses that have been connected to it, and as long as you connect to it, it will tell you all the node information it records. This way you can connect to a node quickly. The seed node will disconnect from you immediately after giving you all the node information, so it is not recommended that the validator node enable the seed mode.  <mark style="color:blue;">**This should be set to**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**`seed_mode=false`**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**.**</mark>** **<mark style="color:red;">**This should be set to seed\_mode=false.**</mark>
* `persistent-peers:` a comma separated list of `nodeID@ip:port` values that define a list of peers that are expected to be online at all times. This is necessary at first startup because by setting `pex=false` the node will not be able to join the network. <mark style="color:blue;">**This should be configured to a list of sentry nodes.**</mark>** **<mark style="color:red;">**This should be configured to your any nodes that you trust, this includes your validator node, the Function X's nodes and other sentry nodes (optional).**</mark>
* `unconditional_peer_ids:` comma separated list of `nodeID`. These nodes will be connected to no matter the limits of inbound and outbound peers. This is useful for when sentry nodes have full address books. <mark style="color:blue;">**It can be filled with sentry node IDs (optional).**</mark>** **<mark style="color:red;">**Validator node ID, optionally sentry node IDs.**</mark>
* `private_peer_ids:` comma separated list of `nodeID`. **These nodes will not be gossiped to the network.** This is an important field as you do not want your validator IP gossiped to the network. <mark style="color:blue;">**This can be left empty as the validator is not trying to hide who it is communicating with.**</mark>** **<mark style="color:red;">**This should be configured to your validator node ID to ensure your validator node's ID is hidden.**</mark>
* `addr_book_strict:` boolean. By default nodes with a routable address will be considered for connection. If this setting is turned off (false), non-routable IP addresses, like addresses in a private network can be added to the address book. <mark style="color:blue;">**This should be set to**</mark><mark style="color:blue;">** **</mark><mark style="color:blue;">**`addr_book_strict=false`**</mark><mark style="color:blue;">.</mark> <mark style="color:red;">**This should be set to**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`addr_book_strict=false`**</mark><mark style="color:red;">.</mark>
* `double-sign-check-height` int64 height. How many blocks to look back to check existence of the node's consensus votes before joining consensus When non-zero, the node will panic upon restart if the same consensus key was used to sign {double\_sign\_check\_height} last blocks. So, validators should stop the state machine, wait for some blocks, and then restart the state machine to avoid panic. <mark style="color:blue;">**This should be configured to 10.**</mark>** **<mark style="color:red;">**This configuration is not important for sentry nodes.**</mark>
* `seeds`**:** Linked to the `seed_mode` configuration mentioned above, a seed node is a special node that allows the incorporation of new nodes to the network and maintains the strength of the network at all times, by allowing them to synchronize and obtain a copy of the data from the blockchain, replicating it and adding resistance and security to it. For validator nodes <mark style="color:blue;">If you have already configured the</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`persistent-peers`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">to have a list of sentry nodes or nodes that you fully trust, then you can leave the</mark> <mark style="color:blue;"></mark><mark style="color:blue;">`seeds`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">field empty.</mark> <mark style="color:red;">This</mark> <mark style="color:red;"></mark><mark style="color:red;">`seeds`</mark> <mark style="color:red;"></mark><mark style="color:red;">field should be configured to locate other peers.</mark>

**Validator Node Configuration**

| Config Option                       | Setting                    |
| ----------------------------------- | -------------------------- |
| seed\_mode                          | false                      |
| pex                                 | false                      |
| persistent-peers (`nodeID@ip:port`) | list of sentry nodes       |
| private-peer-ids (`nodeID`)         | none                       |
| unconditional-peer-ids (`nodeID`)   | optionally sentry node IDs |
| addr-book-strict                    | false                      |
| double-sign-check-height            | 10                         |

**Sentry Node Configuration**

| Config Option                       | Setting                                       |
| ----------------------------------- | --------------------------------------------- |
| seed\_mode                          | false                                         |
| pex                                 | true                                          |
| persistent-peers (`nodeID@ip:port`) | validator node, optionally other sentry nodes |
| private-peer-ids (`nodeID`)         | validator node ID                             |
| unconditional-peer-ids (`nodeID`)   | validator node ID, optionally sentry node IDs |
| addr-book-strict                    | false                                         |

### To obtain your node ID

Run this command:

```
fxcored tendermint show-node-id
```

{% hint style="info" %}
For those fields that require just a NodeID, it will be something similar to (example):

6f55c84d12klmsdf7c4be0dc15b667892aaeea5cd7

For those fields that require `nodeID@ip:port` (persistent-peers), it will be something similar to (example):

6f55c84d12klmsdf7c4be0dc15b667892aaeea5cd7@44.01.22.33:26656
{% endhint %}
