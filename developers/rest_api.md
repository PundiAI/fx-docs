# f(x)Core REST API & GRPC

### Methods and endpoints additional resources

There are many endpoints and methods for REST API. Since f(x)Core is built on cosmos-sdk, some existing cosmos endpoints would be exposed as well. You may find a list of

* [Cosmos SDK - Legacy REST and gRPC Gateway docs](https://v1.cosmos.network/rpc/v0.42.6)
* [fx-core\_rest\_and\_grpc\_gateway\_docs](https://app.swaggerhub.com/apis/functionx/fx-core\_rest\_and\_g\_rpc\_gateway\_docs/1.0.0#/)

Depending on the type of requests, you may specify the header, parameters and body, but generally a query should look like the following:

```
curl <node_endpoint>/<path>
```

**For example:**

**Request path:** `/cosmos/bank/v1beta1/supply`

**Method: `GET`**

```
curl https://fx-rest.functionx.io/cosmos/bank/v1beta1/supply
```

<details>

<summary>Response</summary>

```shell
{
  "supply": [
    {
      "denom": "FX",
      "amount": "496223453338977974284188508"
    },
    {
      "denom": "eth0x0FD10b9899882a6f2fcb5c371E17e70FdEe00C38",
      "amount": "28994724975171599793484405"
    },
    {
      "denom": "ibc/F08B62C2C1BE9E52942617489CAB1E94537FE3849F8EEC910B142468C340EB0D",
      "amount": "222957280749898486123894"
    },
    {
      "denom": "polygon0xc2132D05D31c914a87C6611C10748AEb04B58e8F",
      "amount": "20131249"
    },
    {
      "denom": "tronTR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
      "amount": "195780912"
    }
  ]
}
```

</details>

### Other examples:

**Requesting the details of the address**

#### Request **Path:** `/cosmos/auth/v1beta1/accounts/{address}`

**Method `GET`**

**Path Parameters:** address(string) -- f(x)Core address (example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

<details>

<summary>Response</summary>

```shell
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

</details>

#### Requesting the balance of the address

#### Request Path: ** `/cosmos/bank/v1beta1/balances/{address}`**

**Method `GET`**

**Path Parameters:** address(string) -- f(x)Core address(example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

<details>

<summary>Response</summary>

```shell
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

</details>

#### Requesting the current token supply of $FX token

* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX
  * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
  * Non-delegated asset $FX = Ethereum cross chain locked fund + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
  * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum

#### Request Path: ** `/cosmos/bank/v1beta1/supply`**

**Method `GET`**

**Path Parameters:** None

<details>

<summary>Response</summary>

```shell
{
  "supply": [
    {
      "denom": "FX",
      "amount": "438724604960126741923659830"
    }
  ]
}
```

</details>
