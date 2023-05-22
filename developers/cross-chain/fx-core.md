# f(x)Core Cross Chain

use cross chain precompiled contract, to realize the cross chain of token

f(x)Core cross chain needs to use cross chain precompiled contract, please check
the [Cross Chain](../precompiles/cross-chain.md#crossChain) document first

## Tokens

f(x)Core cross chain supports the following tokens:

{% tabs %}
{% tab title="Mainnet" %}

| Token                                                                              | Address                                    |
|:-----------------------------------------------------------------------------------|:-------------------------------------------|
| [WFX](https://starscan.io/evm/token/0x80b5a32E4F032B2a058b4F29EC95EEfEEB87aDcd)    | 0x80b5a32E4F032B2a058b4F29EC95EEfEEB87aDcd |
| [PUNDIX](https://starscan.io/evm/token/0xd567B3d7B8FE3C79a1AD8dA978812cfC4Fa05e75) | 0xd567B3d7B8FE3C79a1AD8dA978812cfC4Fa05e75 |
| [PURSE](https://starscan.io/evm/token/0x5FD55A1B9FC24967C4dB09C513C3BA0DFa7FF687)  | 0x5FD55A1B9FC24967C4dB09C513C3BA0DFa7FF687 |
| [USDT](https://starscan.io/evm/token/0xecEEEfCEE421D8062EF8d6b4D814efe4dc898265)   | 0xecEEEfCEE421D8062EF8d6b4D814efe4dc898265 |
| [WETH](https://starscan.io/evm/token/0x0CE35b0D42608Ca54Eb7bcc8044f7087C18E7717)   | 0x0CE35b0D42608Ca54Eb7bcc8044f7087C18E7717 |
| [DAI](https://starscan.io/evm/token/0x1D54EcB8583Ca25895c512A8308389fFD581F9c9)    | 0x1D54EcB8583Ca25895c512A8308389fFD581F9c9 |
| [USDC](https://starscan.io/evm/token/0x5db67696C3c088DfBf588d3dd849f44266ff0ffa)   | 0x5db67696C3c088DfBf588d3dd849f44266ff0ffa |
| [WBTC](https://starscan.io/evm/token/0x13974cf36984216C90D1F4FC815C156092feA396)   | 0x13974cf36984216C90D1F4FC815C156092feA396 |
| [EURS](https://starscan.io/evm/token/0xc03345448969Dd8C00e9E4A85d2d9722d093aF8E)   | 0xc03345448969Dd8C00e9E4A85d2d9722d093aF8E |
| [LINK](https://starscan.io/evm/token/0xFA3C22C069B9556A4B2f7EcE1Ee3B467909f4864)   | 0xFA3C22C069B9556A4B2f7EcE1Ee3B467909f4864 |
| [C98](https://starscan.io/evm/token/0xC5e00D3b04563950941f7137B5AfA3a534F0D6d6)    | 0xC5e00D3b04563950941f7137B5AfA3a534F0D6d6 |
| [BUSD](https://starscan.io/evm/token/0x5aD523d94Efb56C400941eb6F34393b84c75ba39)   | 0x5aD523d94Efb56C400941eb6F34393b84c75ba39 |
| [FRAX](https://starscan.io/evm/token/0x3452e23F9c4cC62c70B7ADAd699B264AF3549C19)   | 0x3452e23F9c4cC62c70B7ADAd699B264AF3549C19 |
| [WAVAX](https://starscan.io/evm/token/0x15C3Eb3B621d1Bff62CbA1c9536B7c1AE9149b57)  | 0x15C3Eb3B621d1Bff62CbA1c9536B7c1AE9149b57 |
| [sAVAX](https://starscan.io/evm/token/0xF5b24c0093b65408ACE53df7ce86a02448d53b25)  | 0xF5b24c0093b65408ACE53df7ce86a02448d53b25 |
| [QI](https://starscan.io/evm/token/0x50dE24B3f0B3136C50FA8A3B8ebc8BD80a269ce5)     | 0x50dE24B3f0B3136C50FA8A3B8ebc8BD80a269ce5 |
| [BAVA](https://starscan.io/evm/token/0xc8B4d3e67238e38B20d38908646fF6F4F48De5EC)   | 0xc8B4d3e67238e38B20d38908646fF6F4F48De5EC |

{% endtab %}

{% tab title="Testnet" %}

| Token                                                                                      | Address                                    |
|:-------------------------------------------------------------------------------------------|:-------------------------------------------|
| [WFX](https://testnet.starscan.io/evm/token/0x3452e23F9c4cC62c70B7ADAd699B264AF3549C19)    | 0x3452e23F9c4cC62c70B7ADAd699B264AF3549C19 |
| [PUNDIX](https://testnet.starscan.io/evm/token/0x5db67696C3c088DfBf588d3dd849f44266ff0ffa) | 0x5db67696C3c088DfBf588d3dd849f44266ff0ffa |
| [PURSE](https://testnet.starscan.io/evm/token/0xc8B4d3e67238e38B20d38908646fF6F4F48De5EC)  | 0xc8B4d3e67238e38B20d38908646fF6F4F48De5EC |
| [USDT](https://testnet.starscan.io/evm/token/0x3515F25AB7637adcF1b69F4D384ed5936B83431F)   | 0x3515F25AB7637adcF1b69F4D384ed5936B83431F |
| [WETH](https://testnet.starscan.io/evm/token/0xA2A4B12EF81E7A26C5a1E0be9340b1972F85E44A)   | 0xA2A4B12EF81E7A26C5a1E0be9340b1972F85E44A |
| [WBTC](https://testnet.starscan.io/evm/token/0x8FA78CEB7F04118Ec6d06AaC37Ca854691d8e963)   | 0x8FA78CEB7F04118Ec6d06AaC37Ca854691d8e963 |
| [DAI](https://testnet.starscan.io/evm/token/0x4AF57825b86Abf93D4F9D720bEF7c1C1b300A6F3)    | 0x4AF57825b86Abf93D4F9D720bEF7c1C1b300A6F3 |
| [USDC](https://testnet.starscan.io/evm/token/0xE60CE2dfa6D4Ad37Ade1dcB7aC4D6C3A093b3A7E)   | 0xE60CE2dfa6D4Ad37Ade1dcB7aC4D6C3A093b3A7E |
| [WAVAX](https://testnet.starscan.io/evm/token/0x2C68D1d6aB986Ff4640b51e1F14C716a076E44C4)  | 0x2C68D1d6aB986Ff4640b51e1F14C716a076E44C4 |
| [EURS](https://testnet.starscan.io/evm/token/0xB5124FA2b2cF92B2D469b249433BA1c96BDF536D)   | 0xB5124FA2b2cF92B2D469b249433BA1c96BDF536D |
| [LINK](https://testnet.starscan.io/evm/token/0xF0965c8f0755CF080a61C91EDd6707F0532c8fE7)   | 0xF0965c8f0755CF080a61C91EDd6707F0532c8fE7 |
| [C98](https://testnet.starscan.io/evm/token0x9d6F2a9fDB32708e1AC07788cc29D6125ac73027/)    | 0x9d6F2a9fDB32708e1AC07788cc29D6125ac73027 |
| [sAVAX](https://testnet.starscan.io/evm/token/0xD32eB974468ed767338533842D2D4Cc90B9BAb46)  | 0xD32eB974468ed767338533842D2D4Cc90B9BAb46 |
| [QI](https://testnet.starscan.io/evm/token/0xc71aAf8e486e3F33841BB56Ca3FD2aC3fa8D29a8)     | 0xc71aAf8e486e3F33841BB56Ca3FD2aC3fa8D29a8 |
| [BAVA](https://testnet.starscan.io/evm/token/0xc7e56EEc629D3728fE41baCa2f6BFc502096f94E)   | 0xc7e56EEc629D3728fE41baCa2f6BFc502096f94E |

{% endtab %}
{% endtabs %}

## crossChain

f(x)Core token send to other chain, should use [crossChain](../precompiles/cross-chain.md#crosschain) method

### 1. Approve token

call `approve` method in token contract, approve `amount + fee`
to [CrossChain contract address](../precompiles/cross-chain.md)

{% hint style="info" %}
if token if original token, not need approve
{% endhint %}

### 2. CrossChain token

call `crossChain` method in CrossChain contract, pass the corresponding parameters

{% hint style="info" %}
if token if original token, crossChain _token is zero address , crossChain msg.value must equal `amount + fee`
{% endhint %}


