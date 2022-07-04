# FunctionX Technical Document

FunctionX Technical Document will cover the areas of f(x)Core command line interface, information on validators and delegators; technical set up, consensus, governance, security management etc. Also there are additional resources for users and general best practices within the document.

Additionally, there are developer commands and documents for querying the f(x)Core blockchain.

### F(X)CORE

[**What is f(x)Core**](f-x-core/what-is-f-x-core.md): Brief overview of f(x)Core including the various modules.

****[**Installation f(x)Core**](f-x-core/installation.md) **(technical):** The hardware requirements and the technical setup for the f(x)Core command line interface (CLI).

[**Setup Node**](f-x-core/setup-node/) **(technical)**

* [Full node with Binaries](f-x-core/setup-node/full-node-with-binaries.md): Initialising the core (where your consensus keys are generated), configuring the necessary files, running the node with [Daemon](f-x-core/setup-node/full-node-with-binaries.md#running-server-as-a-daemon).
* [Full node with Docker](f-x-core/setup-node/full-node-with-docker.md): Similar to setting it up using binaries, but using the docker container.
* [Snapshot Guide](f-x-core/setup-node/use-snapshot.md): Downloading a snapshot of the blockchain data to enable a faster sync.
* [Node Metrics](f-x-core/setup-node/node-monitor.md): Setting up of [Prometheus monitoring services](f-x-core/setup-node/node-monitor.md#prometheus-metrics) and also [creating alerts for telegram](f-x-core/setup-node/node-monitor.md#telegram-administrator-and-bot-configuration).
* [Node Peers](f-x-core/setup-node/node-peers.md): Trustable nodes.
* [EVM Upgrade Tutorial](evm-upgrade/evm-upgrade-tutorial.md): How to upgrade your node's code to be compatible with the latest EVM module

### VALIDATORS

[**Validator Overview**](validators/validator-overview.md): General overview and links to some of our forums and social media accounts.

****[**Setting Up a Validator for f(x)Core**](validators/validator-setup.md) **(technical):** [Creation of validator](validators/validator-setup.md#create-your-validator), [edit validator description](validators/validator-setup.md#edit-validator-description), [unjailing your validator](validators/validator-setup.md#edit-validator-description) and some [common problems](validators/validator-setup.md#common-problems).

****[**Validator Recovery**](validators/validator-recovery.md) **(technical):** Where your consensus key is stored and how to to do a recovery.

[**Validator FAQ**](validators/validator-faq.md): [General concepts](validators/validator-faq.md#general-concepts) like [what is a full-node](validators/validator-faq.md#what-is-a-validator), [what is a validator](validators/validator-faq.md#what-is-a-validator), [what is a delegator](validators/validator-faq.md#what-is-a-delegator), [What are the different states a validator can be in?](validators/validator-faq.md#what-are-the-different-states-a-validator-can-be-in), [responsibilities](validators/validator-faq.md#responsibilities), [what are the slashing.conditions?](validators/validator-faq.md#what-are-the-slashing-conditions), [incentives](validators/validator-faq.md#incentives)...

[**Validator Security Notice**](validators/validator-security-notice.md): [Key Management - HSM](validators/validator-security-notice.md#key-management-hsm), [Sentry Nodes (DDOS Protection)](validators/validator-security-notice.md#sentry-nodes-ddos-protection).

### DELEGATORS

[**Delegator FAQ**](delegators/delegators-faq.md): [What is a delegator?](delegators/delegators-faq.md#what-is-a-delegator), [choosing a validator](delegators/delegators-faq.md#choosing-a-validator), [directives of delegators](delegators/delegators-faq.md#directives-of-delegators), [revenue](delegators/delegators-faq.md#revenue), [risks](delegators/delegators-faq.md#risks).

****[**Delegator CLI Guide**](delegators/delegator-cli-guide.md) **(technical):** [f(x)Core Accounts](delegators/delegator-cli-guide.md#f-x-core-accounts), [querying the state](delegators/delegator-cli-guide.md#querying-the-state), [sending transactions](delegators/delegator-cli-guide.md#sending-transactions), [Bonding FX and Withdrawing Rewards](delegators/delegator-cli-guide.md#bonding-fx-and-withdrawing-rewards), [Participating in Governance](delegators/delegator-cli-guide.md#participating-in-governance).

[**Delegator Security Notice**](delegators/delegator-security-notice.md): [Social Engineering](delegators/delegator-security-notice.md#social-engineering), [Key Management](delegators/delegator-security-notice.md#key-management), [Software Vulnerabilities](delegators/delegator-security-notice.md#software-vulnerabilities), [Account Security](delegators/delegator-security-notice.md#account-security).

### RESOURCES

[**Cloud setup**](resources/cloud-setup.md): how to remote ssh and [connecting your localhost to the cloud instance for a specific port](resources/cloud-setup.md#connecting-your-localhost-to-the-cloud-instance-for-a-specific-port).

[**Testnet faucet**](resources/fxtestnetfaucet.md)**:** sorry, there isn't one for Mainnet.

[**f(x)cored Commands Documentation**](resources/f-x-cored-commands-documentation.md): [Main structure of running fxcored commands](resources/f-x-cored-commands-documentation.md#main-structure-of-running-fxcored-commands), [keys](resources/f-x-cored-commands-documentation.md#keys), [send tokens](resources/f-x-cored-commands-documentation.md#send-tokens), [query transactions](resources/f-x-cored-commands-documentation.md#query-transactions), [slashing](resources/f-x-cored-commands-documentation.md#slashing), [staking](resources/f-x-cored-commands-documentation.md#staking), [governance](resources/f-x-cored-commands-documentation.md#governance), [fee distribution](resources/f-x-cored-commands-documentation.md#fee-distribution), [multisig transactions](resources/f-x-cored-commands-documentation.md#multisig-transactions).

[**Ledger Integration for fxcored**](resources/ledger-integration-for-fxcored.md): [Installing cosmos ledger application](resources/ledger-integration-for-fxcored.md#install-the-cosmos-ledger-application), [adding keys to your ledger using f(x)Core CLI](resources/ledger-integration-for-fxcored.md#f-x-core-cli-+-ledger-nano).

### STEP BY STEP SUMMARY OF HOW TO SET UP A VALIDATOR

[**Installation f(x)Core**](f-x-core/installation.md)

1. Install the necessary dependencies and [install Go](f-x-core/installation.md#install-go)
2. [Install the binaries](f-x-core/installation.md#install-go)

[**Setup Node**](f-x-core/setup-node/) **(you can choose to set it up using** [**Binaries**](f-x-core/setup-node/full-node-with-binaries.md) **or** [**Docker**](f-x-core/setup-node/full-node-with-docker.md)**)**

1. Initialize fxcore
2. Make sure you [backup your consensus key](validators/validator-recovery.md)❗
3. Before starting the node, you can choose to [download the snapshot](f-x-core/setup-node/use-snapshot.md) (recommended)
4. Start the node to sync, [running server as a daemon](f-x-core/setup-node/full-node-with-binaries.md#running-server-as-a-daemon) is highly recommended

[**Setting Up a Validator for f(x)Core**](validators/validator-setup.md)

1. Ensure your node is synced up, "catch\_up"=false
2. You can use [ledger](resources/ledger-integration-for-fxcored.md) to create your token holding account keys and add a layer of protection by using the `keyring-backend` flag (highly recommended), you can even use a multi-sig account but that will make transactions a hassle later, and do some test transactions to make sure everything works
3. Make sure you do the necessary checks before you create your validator❗
4. Also set up your [node monitoring device](f-x-core/setup-node/node-monitor.md) on a separate server
5. [Create your validator](validators/validator-setup.md#create-your-validator)
