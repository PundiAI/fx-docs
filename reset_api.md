# f(x)Core REST API 

### Requesting the details of the address 

#### Request

##### Query Path `/cosmos/auth/v1beta1/accounts/{address}`

##### Request Method `GET`

##### Query Parameters

* address(string): f(x)Core address (example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

#### Response

* 200 Successful
    
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

#### Request

##### Query Path `/cosmos/bank/v1beta1/balances/{address}`

##### Request Method `GET`

##### Query Parameters

* address(string): f(x)Core address(example: fx1zgpzdf2uqla7hkx85wnn4p2r3duwqzd8xst6v2)

#### Response

* 200 Successful

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

* Total circulating supply of $FX = Delegated asset $FX + Non-delegated asset $FX 
    * Delegated asset $FX = Total $FX that delegated in f(x)Core validator node
    * Non-delegated asset $FX = Ethereum cross chain locked fund  + Unclaimed reward of validator (including commission and transaction fee) + Unclaimed reward of delegator + Wallet balance + Pool of ecosystem and community + Locked fund of Governance
    * Ethereum cross chain locked fund = Total $FX (ERC20) on Ethereum

#### Request

##### Query Path `/cosmos/bank/v1beta1/supply`

##### Request Method `GET`

##### Query Parameters

None

#### Response

* 200 Successful

    ```json
    {
      "supply": [
        {
          "denom": "FX",
          "amount": "438724604960126741923659830"
        }
      ]
    }
    ````
