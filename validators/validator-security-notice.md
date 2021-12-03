# Validator Security Notice

Each validator is encouraged to run its operations independently, as diverse setups increase the resilience of the network. Validators should be well versed in the setup and maintenance process of a validator to ensure security and stability of their individual validators and the network as a whole.

## Key Management - HSM

It is critical that an attacker cannot steal a validator's keys. If this is possible, it compromises the validator's staked assets as well as all its delegated stake. Hardware security modules are an important strategy for mitigating this risk.

HSM modules must support `ed25519` signatures for the hub. The FunctionX team is also working on extending our Ledger Nano X application to support validator signing. This app can store recent blocks and mitigate double signing attacks.

We will update this page when more key storage solutions become available.

## Sentry Nodes (DDOS Protection)

Validators are responsible for ensuring that the network can sustain denial of service attacks.

One recommended way to mitigate these risks is for validators to carefully structure their network topology in a so-called sentry node architecture.

Validator nodes should only connect to full-nodes they trust because they operate them themselves or are run by other validators they can trust or know socially. A validator node will typically run in a data center. Most data centers provide direct links to the networks of major cloud providers. The validator can use those links to connect to sentry nodes in the cloud. This shifts the attack vector of denial-of-service from the validator's node directly to its sentry nodes. This may require new sentry nodes be spun up or activated on the fly to mitigate attacks on existing ones.

Sentry nodes can be spun up quickly or have their IP addresses changed. Because the links to the sentry nodes are in private IP space, an internet based attacked will not affect them directly. This will ensure validator block proposals and votes will always be broadcasted to the rest of the network.

To setup your sentry node architecture you can follow the instructions below:

The config.toml file should be stored in .fxcore/config/config.toml file path. You may run the `vi` command line editor to edit the config.toml file. If you were in your root directory where .fxcore is situated, you may run the following command to launch the config.toml file using the Vi command line editor:

```
vi .fxcore/config/config.toml
```


Validators nodes should edit their config.toml:

```bash
# Comma separated list of nodes to keep persistent connections to
# Do not add private peers to this list if you don't want them advertised
# Example ID: 3e16af0cead27979e1fc3dac57d03df3c7a77acc@node-2.functionx.io:26656
persistent_peers = "list of sentry nodes"

# Set true to enable the peer-exchange reactor
pex = false
```

Sentry Nodes should edit their config.toml:

```bash
# Comma separated list of peer IDs to keep private (will not be gossiped to other peers)
# Example ID: 3e16af0cead27979e1fc3dac57d03df3c7a77acc@3.87.179.235:26656

private_peer_ids = "node_ids_of_private_peers"
```

Once you have made changes and want to save the file, press `:x` (you must not be in Insert mode. If you are, press ESC to leave it).

## Ports
On the note DDOS, remember to ensure you have closed off the ports that should not be made public. You may consider whitelisting the addresses that need access to certain ports that need to be open but not made public.

## Environment Variables

Note that while explicit command-line flags will take precedence over environment variables, environment variables will take precedence over any of your configuration files. For this reason, it's imperative that you lock down your environment such that any critical parameters are defined as flags on the CLI or prevent modification of any environment variables.
