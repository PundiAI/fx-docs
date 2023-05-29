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

| source chain-id | source channel | destination             | destination chain-id    | destination channel |
|:----------------|:---------------|:------------------------|:------------------------|:--------------------|
| fxcore          | channel-0      | Pundix                  | PUNDIX                  | channel-0           |
| fxcore          | channel-0      | Marginx-APE             | Marginx-APE             | channel-3           |
| fxcore          | channel-0      | Marginx-BTC             | Marginx-BTC             | channel-4           |
| fxcore          | channel-0      | Marginx-ETH             | Marginx-ETH             | channel-5           |
| fxcore          | channel-0      | Marginx-FX              | Marginx-FX              | channel-6           |
| fxcore          | channel-0      | MarginX-MONGUSDT        | MarginX-MONGUSDT        | channel-7           |
| fxcore          | channel-0      | MarginX-PIZAUSDT        | MarginX-PIZAUSDT        | channel-8           |
| fxcore          | channel-0      | MarginX-1000000TLIPUSDT | MarginX-1000000TLIPUSDT | channel-9           |

{% endtab %}
{% tab title="Testnet" %}

| source chain-id | source channel | destination | destination chain-id | destination channel |
|:----------------|:---------------|:------------|:---------------------|:--------------------|
| dhobyghaut      | channel-0      | Pundix      | payalebar            | channel-0           |
| dhobyghaut      | channel-0      | DEX-AAPL    | DEX-AAPL             | channel-82          |
| dhobyghaut      | channel-0      | DEX-BTC     | DEX-BTC              | channel-83          |
| dhobyghaut      | channel-0      | DEX-BTC     | DEX-BTC              | channel-84          |
| dhobyghaut      | channel-0      | DEX-TSLA    | DEX-TSLA             | channel-85          |
| dhobyghaut      | channel-0      | DEX-APT     | DEX-APT              | channel-86          |
| dhobyghaut      | channel-0      | DEX-MONG    | DEX-MONG             | channel-87          |
| dhobyghaut      | channel-71     | Osmosis     | osmo-test-5          | channel-88          |
| dhobyghaut      | channel-0      | DEX-ORDS    | DEX-ORDS             | channel-89          |
| dhobyghaut      | channel-2631   | Cosmos      | theta-testnet-001    | channel-90          |
| dhobyghaut      | channel-0      | DEX-PIZA    | DEX-PIZA             | channel-91          |

{% endtab %}
{% endtabs %}