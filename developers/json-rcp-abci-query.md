# f(x)Core JSON RPC ABCI Query

{% hint style="danger" %}
The content under the ABCI Query custom module (including the cosmos-sdk module) may not be supported eventually. It is not recommended for users to use. Users are recommended instead to use [**REST API or gRPC**](rest\_api.md)**.**
{% endhint %}

### Requesting the details of the address

* JSON RPC method: `abci_query`
* JSON RPC params path: `custom/auth/account`
* JSON RPC params data: input json `{"address":"fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2"}` hex code
* request body:

```json
{
    "jsonrpc":"2.0",
    "id":"client-12345678",
    "method":"abci_query",
    "params":{
        "path":"custom/auth/account",
        "data":"7b2261646472657373223a226678317a67707a64663275716c6137686b783835776e6e3470327233647577717a6438787374367632227d",
        "height":"0",
        "prove":false
    }
}
```

* response body:

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "response": {
      "code": 0,
      "log": "",
      "info": "",
      "index": "0",
      "key": null,
      "value": "ewogICJ0eXBlIjogImNvc21vcy1zZGsvQmFzZUFjY291bnQiLAogICJ2YWx1ZSI6IHsKICAgICJhZGRyZXNzIjogImZ4MXpncHpkZjJ1cWxhN2hreDg1d25uNHAycjNkdXdxemQ4eHN0NnYyIiwKICAgICJwdWJsaWNfa2V5IjogewogICAgICAidHlwZSI6ICJ0ZW5kZXJtaW50L1B1YktleVNlY3AyNTZrMSIsCiAgICAgICJ2YWx1ZSI6ICJBOU8vYXJiM1dDS3lsQ2hoVnhmQTFJRlhCbmZpOE50QU1kbm9SOUg1VmxBcyIKICAgIH0sCiAgICAic2VxdWVuY2UiOiAiMSIKICB9Cn0=",
      "proofOps": null,
      "height": "2185",
      "codespace": ""
    }
  }
}
```

* decrypt the data in "value" by using base 64 decryption code to get the data as follows:

```json
{
    "type": "cosmos-sdk/BaseAccount",
    "value": {
      "address": "fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2",
      "public_key": {
        "type": "tendermint/PubKeySecp256k1",
        "value": "A9O/arb3WCKylChhVxfA1IFXBnfi8NtAMdnoR9H5VlAs"
      },
      "sequence": "1",
      "account_number": "0"
    }
}
```

### Requesting the balance of the address

* JSON RPC method: `abci_query`
* JSON RPC params path: `custom/bank/all_balances`
* JSON RPC params data: input json`{"address":"fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2"}` hex code
* request body:

```json
{
    "jsonrpc":"2.0",
    "id":"client-12345678",
    "method":"abci_query",
    "params":{
        "path":"custom/auth/account",
        "data":"7b2261646472657373223a226678317a67707a64663275716c6137686b783835776e6e3470327233647577717a6438787374367632227d",
        "height":"0",
        "prove":false
    }
}
```

* response body:

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "response": {
      "code": 0,
      "log": "",
      "info": "",
      "index": "0",
      "key": null,
      "value": "WwogIHsKICAgICJkZW5vbSI6ICJGWCIsCiAgICAiYW1vdW50IjogIjI5OTk5MDAwMDAwMDAwMDAwMDAwMDAwMDAiCiAgfQpd",
      "proofOps": null,
      "height": "2185",
      "codespace": ""
    }
  }
}
```

* decrypt the data in "value" by using base 64 decryption code to get the data as follows:

```json
[
  {
    "denom": "FX",
    "amount": "2999900000000000000000000"
  }
]
```

### Requesting the total supply of $FX token

* JSON RPC method: `abci_query`
* JSON RPC params path: `custom/bank/supply_of`
* JSON RPC params data: input json `{"Denom":"FX"}` hex code
* request body:

```json
{
    "jsonrpc":"2.0",
    "id":0,
    "method":"abci_query",
    "params":{
        "data":"7b2244656e6f6d223a224658227d",
        "height":"0",
        "path":"custom/bank/supply_of",
        "prove":true
    }
}
```

* response body:

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "response": {
      "code": 0,
      "log": "",
      "info": "",
      "index": "0",
      "key": null,
      "value": "ewogICJkZW5vbSI6ICJGWCIsCiAgImFtb3VudCI6ICI0Mzg2NTgxMzM1MjgyODQ4NDMxMDQzMDMxODEiCn0",
      "proofOps": null,
      "height": "2185",
      "codespace": ""
    }
  }
}
```

* decrypt the data in "value" by using base 64 decryption code to get the data as follows:

```json
{
  "denom": "FX",
  "amount": "438658133528284843104303181"
}
```
