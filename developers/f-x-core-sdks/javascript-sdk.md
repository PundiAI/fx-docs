# JavaScript SDK

GitHub: [https://github.com/FunctionX/fx-js-sdk](https://github.com/FunctionX/fx-js-sdk)

fxwallet version (minimum): `v2.0`

WalletConnectId: 39778

CHAIN\_ID: fxevm

## Overview

Like the python SDK, Function X's JavaScript SDK: `fx-js-sdk`, can also be used to prototype, develop, and deploy blockchain solutions using JavaScript.

## Usage Examples

### Get Accounts

{% code lineNumbers="true" %}
```javascript
export function getAccountRequest(chainIds) {
  return {
    id: payloadId(),
    jsonrpc: '2.0',
    method: 'functionx_wc_accounts_v1',
    params: chainIds,
  };
}
```
{% endcode %}

#### Example

{% code lineNumbers="true" %}
```javascript
const request = getAccountRequest([CHAIN_ID,CHAIN_ID]);
connector.sendCustomRequest(request)
  .then((accounts) => {
    setAccounts(accounts);
    console.log(accounts.length == 1);
  }).catch((error) => {
    console.error(error);
  });
```
{% endcode %}

#### Result

`[name, algo, publicKey, addressByte, bech32Address]`

### Sign Transactions (amino)

Sign transaction using FunctionX Mobile Wallet via Wallet Connect

* signer: `bech32Address`

#### Example

{% code lineNumbers="true" %}
```javascript
import {
  makeSignDoc as makeAminoSignDoc,
  serializeSignDoc
} from "@cosmjs/amino"
import { fromUtf8 } from "@cosmjs/encoding"

const signDoc = makeAminoSignDoc(messages, fee, chainId, memo, accountNumber, sequence)
const signBytes = serializeSignDoc(signDoc)
const signData = fromUtf8(signBytes)
     
connector.sendCustomRequest({
    id: payloadId(),
    jsonrpc: '2.0',
    method: 'functionx_wc_sign_tx_v1',
    params: [chainId, signer, signData],
  })
  .then((response) => {
    const signed = _.get(response, '0.signed');
    const signature = _.get(response, '0.signature');
    return broadcastTx(signed, signature);
  }).then((result) => {
    const code = _.get(result, 'code');
    if (code === 0) {
      const txHash = _.get(result, 'txhash');
      console.log(txHash);
    } else {
      const rawLog = _.get(result, 'raw_log');
      console.error(rawLog);
    }
  })
```
{% endcode %}
