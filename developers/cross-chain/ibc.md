# IBC

In IBC, blockchains do not directly pass messages to each other over the network. This is where relayer comes in. A
relayer process monitors for updates on opens paths between sets of [IBC](https://ibcprotocol.org/) enabled chains. The
relayer submits these updates in the form of specific message types to the counterparty chain. Clients are then used to
track and verify the consensus state.

In addition to relaying packets, this relayer can open paths across chains, thus creating clients, connections and
channels.

Additional information on how IBC works can be found [here](https://ibc.cosmos.network/).

## IBC Channels

{% tabs %}
{% tab title="Mainnet" %}

| source chain-id | destination             | destination chain-id    | source to destination channel | destination to source channel |
|:----------------|:------------------------|:------------------------|:------------------------------|:------------------------------|
| fxcore          | Pundix                  | PUNDIX                  | channel-0                     | channel-0                     |
| fxcore          | Marginx-APE             | Marginx-APE             | channel-3                     | channel-0                     |
| fxcore          | Marginx-BTC             | Marginx-BTC             | channel-4                     | channel-0                     |
| fxcore          | Marginx-ETH             | Marginx-ETH             | channel-5                     | channel-0                     |
| fxcore          | Marginx-FX              | Marginx-FX              | channel-6                     | channel-0                     |
| fxcore          | MarginX-MONGUSDT        | MarginX-MONGUSDT        | channel-7                     | channel-0                     |
| fxcore          | MarginX-PIZAUSDT        | MarginX-PIZAUSDT        | channel-8                     | channel-0                     |
| fxcore          | MarginX-1000000TLIPUSDT | MarginX-1000000TLIPUSDT | channel-9                     | channel-0                     |
| fxcore          | CosmosHub               | cosmoshub-4             | channel-10                    | channel-585                   |
| fxcore          | Osmosis                 | osmosis-1               | channel-19                    | channel-2716                  |
| CosmosHub       | Osmosis                 | osmosis-1               | channel-141                   | channel-0                     |

{% endtab %}
{% tab title="Testnet" %}

| source chain-id   | destination | destination chain-id    | source to destination channel | destination to source channel |
|:------------------|:------------|:------------------------|:------------------------------|:------------------------------|
| dhobyghaut        | Pundix      | payalebar               | channel-0                     | channel-0                     |
| dhobyghaut        | DEX-AAPL    | DEX-AAPL                | channel-82                    | channel-0                     |
| dhobyghaut        | DEX-BTC     | DEX-BTC                 | channel-83                    | channel-0                     |
| dhobyghaut        | DEX-FX      | DEX-FX                  | channel-84                    | channel-0                     |
| dhobyghaut        | DEX-TSLA    | DEX-TSLA                | channel-85                    | channel-0                     |
| dhobyghaut        | DEX-APT     | DEX-APT                 | channel-86                    | channel-0                     |
| dhobyghaut        | DEX-MONG    | DEX-MONG                | channel-87                    | channel-0                     |
| dhobyghaut        | DEX-ORDS    | DEX-ORDS                | channel-89                    | channel-0                     |
| dhobyghaut        | Cosmos      | theta-testnet-001       | channel-90                    | channel-2631                  |
| dhobyghaut        | DEX-PIZA    | DEX-PIZA                | channel-91                    | channel-0                     |
| dhobyghaut        | Osmosis     | osmo-test-5             | channel-97                    | channel-1528                  |
| theta-testnet-001 | Osmosis     | osmo-test-5             | channel-2971                  | channel-1099                  |
| dhobyghaut        | Axelar      | axelar-testnet-lisbon-3 | channel-118                   | channel-358                   |

{% endtab %}
{% endtabs %}

## IBC Transfer

When transferring from an external cosmos chain to f(x)Core using ibc, if the token is registered in f(x)Core's erc20
module, the receiver address can be filled in with the ethereum address. In this case, the ibc module
automatically converts the ibc token to the ERC20 token and transfers it to the ethereum address in the f(x)Core's evm

## Channel Selection

If the token belongs to the osmosis chain, cross chain to f(x)Core, directly through f(x)Core and osmosis channel, such
as OSMO, ATOM/OSMO;
if it is a token of other chains, it needs to transfer through cosmos-hub, and then cross chain to f(x)Core.
for example EVMOS, first crosses chain to cosmos-hub and then through cosmos-hub crosses chain to f(x)Core
