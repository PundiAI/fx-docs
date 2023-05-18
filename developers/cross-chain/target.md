# Target

the target of the cross chain, such as `eth`, `erc20`, `ibc/0/px` etc.

target specifies the operation after the cross chain, such as converting to ERC20 token, cross chain to other cosmos
chain through ibc, or cross chain to external chain through crosschain module.

## Target Rule

### Empty

if target is empty, the cross chain token will be only processed by the cross chain module, and stay on f(x)Core.

for example:

if user call ethereum bridge contract `sendToFx` method, the target is empty, the token will be transfer to address on
f(x)Core


### Prefix with ibc

if target is prefixed with `ibc`, the token will transfer to other cosmos chain through ibc module.

for example:

if user call ethereum bridge contract `sendToFx` method, the target is `ibc/0/px`, the token will be transfer to address
on pundix chain

if user call f(x)Emv CrossChain contract `crossChain` method, the target is `ibc/0/px`, the token will be transfer to
address on pundix chain

### Module Name

if target equals to module name, the token will process by the module, and send to external chain or convert to f(x)Evm
ERC20 token.

for example:

if user call ethereum bridge contract `sendToFx` method, the target is `erc20`, the token will be transfer to f(x)Evm
ERC20 token.

if user call f(x)Evm CrossChain contract `crossChain` method, the target is `eth`, the token will be transfer to ethereum chain.


## Target Support

* `""` means empty string
* `❌` means not support
* `...` means more
* `-` means no value

### External Chain to f(x)Core/f(x)Evm/Pundix/... with Target

| external-chain | f(x)core | f(x)Evm |  pundix  |           Cosmos-Chain            | Ethereum | BSC | External-Chain |
|:--------------:|:--------:|:-------:|:--------:|:---------------------------------:|:--------:|:---:|:--------------:|
|    Ethereum    |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |
|      BSC       |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |
|    Polygon     |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |
|      Tron      |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |
|   Avalanche    |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |
|      ...       |    ""    |  erc20  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |  ❌  |       ❌        |

{% hint style="info" %}
FX is the native token, when cross chain to fx-core, no need to convert to ERC20, so the target is `""`, if other token,
need to convert to ERC20, so the target is `erc20`
{% endhint %}


### f(x)Evm to External-Chain/Pundix/... with Target

| f(x)Evm | Ethereum | BSC | Polygon | Tron | Avalanche | External-Chain |  Pundix  |           Cosmos-Chain            | f(x)Core | f(x)Evm |
|:-------:|:--------:|:---:|:-------:|:----:|:---------:|:--------------:|:--------:|:---------------------------------:|:--------:|:-------:|
|    -    |   eth    | bsc | polygon | tron | avalanche | {module-name}  | ibc/0/px | ibc/{channel-id}/{address-prefix} |    ❌     |    ❌    |


{% hint style="info" %}
if cross chain to other cosmos-chain, need to specify channel-id and address-prefix in target, for example: cross chain
to cosmos-hub, need to find the channel-id of cosmos-hub on f(x)Core, and the address-prefix of cosmos-hub, then fill in
the target in the format of `ibc/{channel-id}/{address-prefix}`, if the channel-id of cosmos-hub on f(x)Core is `channel-8`,
and the address-prefix is `cosmos`, then the target is `ibc/8/cosmos`
{% endhint %}