# Fx Chain REST API 

### Requesting the details of the address 

#### Parameters

address(string): Fx chain address (example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

#### Request

##### HTTP

```sh
curl http://127.0.0.1:1317/cosmos/auth/v1beta1/accounts/{address}
```

#### Response

```json
{
  "account": {
    "@type": "/cosmos.auth.v1beta1.BaseAccount",
    "address": "fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2",
    "pub_key": {
      "@type": "/cosmos.crypto.secp256k1.PubKey",
      "key": "A9O/arb3WCKylChhVxfA1IFXBnfi8NtAMdnoR9H5VlAs"
    },
    "account_number": "0",
    "sequence": "1"
  }
}
```

### Requesting the balance of the address

#### Parameters

address(string): Fx chain address(example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

#### Request

##### HTTP

```sh
curl http://127.0.0.1:1317/cosmos/bank/v1beta1/balances/{address}
```

#### Response

```json
{
  "balances": [
    {
      "denom": "FX",
      "amount": "2999900000000000000000000"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "1"
  }
}
```

### Requesting the current token supply of $FX token


#### Parameters

None

#### Request

##### HTTP

```sh
curl http://127.0.0.1:1317/cosmos/bank/v1beta1/supply
```

#### Response

```json
{
  "supply": [
    {
      "denom": "FX",
      "amount": "438724604960126741923659830"
    }
  ]
}
```

### Requesting the details of the token


#### Parameters

None

#### Request

##### HTTP

```sh
curl http://127.0.0.1:1317/cosmos/bank/v1beta1/denoms_metadata
```

#### Response

```json
{
  "metadatas": [
    {
      "description": "Function X",
      "denom_units": [
        {
          "denom": "fx",
          "exponent": 0,
          "aliases": [
          ]
        },
        {
          "denom": "FX",
          "exponent": 18,
          "aliases": [
          ]
        }
      ],
      "base": "fx",
      "display": "FX"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "1"
  }
}
```
