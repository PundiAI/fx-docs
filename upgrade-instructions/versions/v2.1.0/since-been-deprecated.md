---
description: >-
  f(x)Core mainnet upgrade V2.1.0 was aborted due to unexpected contradictions
  with working-on updates. V2.1.1 was released to continue the fxv2 upgrade
  proposed to take place at height 5,713,000.
---

# fxCore V2.1.0 (since been deprecated)

### The upgrade contains the following main new features/improvement:

* Tendermint version from v0.34.9 to v0.34.20
* Cosmos-SDK version from v0.42.4 to v.45.5
* IBC version upgrade from cosmos-sdk/x/ibc to ibc-go v3.1.0
* New modules: feegrant, authz, feemarket, evm, erc20, migrate
* Migration modules: auth, bank, distribution, gov, slashing, ibc, crosschain (bsc, polygon, tron)
* Crosschain module upgrade: the original oracle deposit will be automatically entrusted to the validator with the largest power value after the upgrade, the oracle can replace the entrusted validator by itself in the future, oracle needs to manually receive the entrusted reward
* [CLI Breaking Changes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#cli-breaking-changes)
* [API Breaking Changes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#api-breaking-changes)
* [Features](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#features)
* [Bug Fixes](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md#bug-fixes)
* More on the details of the upgrade and changes can be found in [CHANGELOG.MD](https://github.com/FunctionX/fx-core/blob/release/v2.1.x/CHANGELOG.md)

{% hint style="info" %}
For more information on the latest version of the upgrade and the upgrade process, refer to [**fxCore V2.1.1 Upgrade Instructions**](./)****
{% endhint %}
