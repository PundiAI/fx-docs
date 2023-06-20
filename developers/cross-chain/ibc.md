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

{% endtab %}
{% tab title="Testnet" %}

| source chain-id | destination | destination chain-id | source to destination channel | destination to source channel |
|:----------------|:------------|:---------------------|:------------------------------|:------------------------------|
| dhobyghaut      | Pundix      | payalebar            | channel-0                     | channel-0                     |
| dhobyghaut      | DEX-AAPL    | DEX-AAPL             | channel-82                    | channel-0                     |
| dhobyghaut      | DEX-BTC     | DEX-BTC              | channel-83                    | channel-0                     |
| dhobyghaut      | DEX-FX      | DEX-FX               | channel-84                    | channel-0                     |
| dhobyghaut      | DEX-TSLA    | DEX-TSLA             | channel-85                    | channel-0                     |
| dhobyghaut      | DEX-APT     | DEX-APT              | channel-86                    | channel-0                     |
| dhobyghaut      | DEX-MONG    | DEX-MONG             | channel-87                    | channel-0                     |
| dhobyghaut      | Osmosis     | osmo-test-5          | channel-88                    | channel-71                    |
| dhobyghaut      | DEX-ORDS    | DEX-ORDS             | channel-89                    | channel-0                     |
| dhobyghaut      | Cosmos      | theta-testnet-001    | channel-90                    | channel-2631                  |
| dhobyghaut      | DEX-PIZA    | DEX-PIZA             | channel-91                    | channel-0                     |

{% endtab %}
{% endtabs %}