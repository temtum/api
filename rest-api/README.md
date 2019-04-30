# Introduction

Temtum Wallet REST API

# Methods

| Method | Description |
|--------|-------------|
| [create_address](#Create-address) | Generate new address and private key |
| [create_transaction](#Transaction-Create) | Request new transaction creation |
| [send_transaction](#Transaction-Send) | Send already generated transaction |
| [get_transaction](#Get-transaction) | Get transaction by id |
| [get_block_by_hash](#Get-block-by-hash) | Get block by hash |
| [get_block_by_index](#Get-block-by-index) | Get block by index |
| [get_block_last](#Get-last-block) | Get last block in the blockchain |
| [get_balance](#Get-balance) | Get current balance of an address |
| [get_unspents](#Get-unspents) | Get unspent inputs of an address |
| [get_statistic](#Get-statistic) | Get blockchain statisctic |

## Create address

#### POST http://127.0.0.1:3001/address/create

#### About

Generates and returns new random key pair.

#### Input (params)

Empty

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| address | string | Public key |
| privateKey | string | Private key |

#### Example

Generate new key pair

#### Input

```
curl -s -u user:pass -X POST http://127.0.0.1:3001/address/create -H 'Content-Type: application/json' -d '{ }'
```

#### Output

````
{
  "privateKey": "73964b743c1ad587e0e116962e8121ff73f9409fa8e9a30b1b389f8eaf97ff4e",
  "address": "02a757bf4164312d2fc95a54ff9f6b54bc8947a97b7ed744d0777f9075ed5f4c2f"
}
````

## Transaction Create

#### POST http://127.0.0.1:3001/transaction/create

#### About

Generates new transaction using sent fields and adds it to the transaction pool.

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| from | string | Yes | Sender's address |
| to | string | Yes | Recipient's address |
| privateKey | string | Yes | Sender's private key |
| amount | int | Yes | Amount of coins to send. Must be equal or greater than 1 |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| transaction | Transaction | Generated transaction object |

#### Example

Create transaction

#### Input

```
curl -s -u user:pass -X POST http://127.0.0.1:3001/transaction/create -H 'Content-Type: application/json' -d '{
   "from": "02bd3a78c2dddd22650c668e2e18bfbc1743b681f5722b34f012c9b7cd38d3bc9f",
   "to": "034f3115e8f2aa97071951313ed582f64a06bd8bdb23b412507a1ab6c138500de9",
   "privateKey": "601e2e4552bac2109237818808ec4696e2da95744b5badee75a3f54cb78a8900",
   "amount": 1
 }'

```

#### Output

```
{
  "transaction": {
       "type": "regular",
       "txIns": [
           {
               "txOutIndex": 1,
               "txOutId": "db5d5e4d488a7a6c23a88a3bc64993197649a5248560f123d75f6271c8b6c30e",
               "amount": 99576613,
               "address": "02bd3a78c2dddd22650c668e2e18bfbc1743b681f5722b34f012c9b7cd38d3bc9f",
               "signature": "d58a31cce3184a26511bfc92dbde6f884983bb851705fc0036eb80a277a8ee09434a08bd20333952e946ae2f1f3b77b056fabfe91af99ba94c70e4e1683ea13c"
           }
       ],
       "txOuts": [
           {
               "address": "034f3115e8f2aa97071951313ed582f64a06bd8bdb23b412507a1ab6c138500de9",
               "amount": 1
           },
           {
               "address": "02bd3a78c2dddd22650c668e2e18bfbc1743b681f5722b34f012c9b7cd38d3bc9f",
               "amount": 99576612
           }
       ],
       "timestamp": 1554724349,
       "id": "d0c147b68b2acaf6d7ddcf53825611c7b89a5960465a2db5a801e88e28d56c1a"
   }
}
```

## Transaction Send

#### POST http://127.0.0.1:3001/transaction/send

#### About

Adds generated transaction to the transactions pool

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| txHex | string | Yes | Generated transaction hex string |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| transaction | Transaction | Added transaction |

#### Example

Add generated transactin to transactions pool

#### Input

```
curl -s -u user:pass -X POST http://127.0.0.1:3001/transaction/send -H 'Content-Type: application/json' -d '{
   "txHex": "7b2274797065223a22726567756c6172222c227478496e73223a5b5d2c2274784f757473223a5b7b2261646472657373223a2230343831363730666333346432663261303630333336326536646335663430666337393761363230396562326264343337306364626335663031343137663363626535633665306536396464356461373534343330643633643163316330343162396536323464373938383939636563626364626232656634343861613462633939222c22616d6f756e74223a357d2c7b2261646472657373223a2230343861623033336561303164333436633736353630366464643464366166666133303162646462326233323333323162653531383533323736613031383365336333616339393065656136656533653036666663383762393565623436666638623935333034626635613138393037373034643535393730623564346635353830222c22616d6f756e74223a31307d5d2c2274696d657374616d70223a313535343732353637322c226964223a2237353264666230353336373536626264646461373231663036363334613062313739333632356232333834366638646363646536396565363535313331383964227d"
 }'

```

#### Output

```

{
   "transaction": {
       "type": "regular",
       "txIns": [
           {
               "txOutIndex": 1,
               "txOutId": "db5d5e4d488a7a6c23a88a3bc64993197649a5248560f123d75f6271c8b6c30e",
               "amount": 99576613,
               "address": "02bd3a78c2dddd22650c668e2e18bfbc1743b681f5722b34f012c9b7cd38d3bc9f",
               "signature": "d58a31cce3184a26511bfc92dbde6f884983bb851705fc0036eb80a277a8ee09434a08bd20333952e946ae2f1f3b77b056fabfe91af99ba94c70e4e1683ea13c"
           }
       ],
       "txOuts": [
           {
               "address": "034f3115e8f2aa97071951313ed582f64a06bd8bdb23b412507a1ab6c138500de9",
               "amount": 1
           },
           {
               "address": "02bd3a78c2dddd22650c668e2e18bfbc1743b681f5722b34f012c9b7cd38d3bc9f",
               "amount": 99576612
           }
       ],
       "timestamp": 1554724349,
       "id": "d0c147b68b2acaf6d7ddcf53825611c7b89a5960465a2db5a801e88e28d56c1a"
   }
}
```

## Get transaction

#### GET http://127.0.0.1:3001/transaction/:id

#### About

Find transaction by id

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| id | String | Yes | Transaction id |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| transaction | Transaction | Transaction object |

#### Example

Find transaction

#### Input

```
curl -s -X GET http://127.0.0.1:3001/transaction/93f3e2f32f009c8a535d85e4e4e2e063b02ae7d94a2aa30eac8e7574aca5be76 -H 'Content-Type: application/json'
```

#### Output

```
"transaction": {
      "type": "regular",
      "txIns": [
          {
              "txOutIndex": 0,
              "txOutId": "7fe39d6552ea208ce1dae135e5b442a5feb81642c92d4655adf40d2292edd6e0",
              "amount": 100000000,
              "address": "02afa661f8cc8308f0d13a2fbd0714af71b5f0a24b84447f020c336d7a6e65b04d",
              "signature": "c8b714b1d707599a09f176fef5811bb8b5e8fc7f5dd37ffebe4d76461e69494a6569520ad7682a6cd28484b970dde5544ee8860282231fb73341ca85a773aea6"
          }
      ],
      "txOuts": [
          {
              "address": "02d573578c88af3065c9374eacdf15600e7f16057cf452c59c4ec6f98a466d8b33",
              "amount": 1
          },
          {
              "address": "02afa661f8cc8308f0d13a2fbd0714af71b5f0a24b84447f020c336d7a6e65b04d",
              "amount": 99999999
          }
      ],
      "timestamp": 1553787165,
      "id": "93f3e2f32f009c8a535d85e4e4e2e063b02ae7d94a2aa30eac8e7574aca5be76"
}
```

## Get block by hash

#### GET http://127.0.0.1:3001/block/:hash

#### About

Find block by hash

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| hash | String | No | Block hash |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| block | Block | Block object |

#### Example

Find block by hash

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/block/4a02fa6c7f243af4bd07de58c2f80b91437ea07f9ac90b03c68f24e9ba93025a -H 'Content-Type: application/json'
```

#### Output:

```
"block": {
      "index": 2,
      "previousHash": "e95d2bf9976df2f5b40675bedc1c3077a058f6edd3e1631efcec7650de53b551",
      "data": [
          {
              "type": "coinbase",
              "txIns": [
                  {
                      "txOutIndex": 2
                  }
              ],
              "txOuts": [
                  {
                      "address": "",
                      "amount": 0
                  }
              ],
              "timestamp": 1553787151,
              "id": "d02aa933e10c49849a251c7250d1b66ac058bf2ec5f9969621b07537f754aae3"
          }
      ],
      "beaconIndex": 305426,
      "beaconValue": "D9680BB9A47FB25E28801E7CB826B2D2ECCB1A627097D35AB9168809E514B6B2CAC0CA6F3C2B834086963FDF6C42FF62576B9C0A34F01903F0334C905AD6C98D",
      "timestamp": 1553787151,
      "hash": "4a02fa6c7f243af4bd07de58c2f80b91437ea07f9ac90b03c68f24e9ba93025a"
}
```

## Get block by index

#### GET http://127.0.0.1:3001/block/:index

#### About

Find block by hash

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| index | String | No | Block index |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| block | Block | Block object |

#### Example

Find block by index

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/block/2 -H 'Content-Type: application/json'
```

#### Output:

```
"block": {
      "index": 2,
      "previousHash": "e95d2bf9976df2f5b40675bedc1c3077a058f6edd3e1631efcec7650de53b551",
      "data": [
          {
              "type": "coinbase",
              "txIns": [
                  {
                      "txOutIndex": 2
                  }
              ],
              "txOuts": [
                  {
                      "address": "",
                      "amount": 0
                  }
              ],
              "timestamp": 1553787151,
              "id": "d02aa933e10c49849a251c7250d1b66ac058bf2ec5f9969621b07537f754aae3"
          }
      ],
      "beaconIndex": 305426,
      "beaconValue": "D9680BB9A47FB25E28801E7CB826B2D2ECCB1A627097D35AB9168809E514B6B2CAC0CA6F3C2B834086963FDF6C42FF62576B9C0A34F01903F0334C905AD6C98D",
      "timestamp": 1553787151,
      "hash": "4a02fa6c7f243af4bd07de58c2f80b91437ea07f9ac90b03c68f24e9ba93025a"
}
```

## Get last block

#### GET http://127.0.0.1/address/:address/balance

#### About

Get last block

#### Input (params)

Empty

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| block | Block | Block object |

#### Example

Get last block

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/block/last -H 'Content-Type: application/json'
```

#### Output:

```
"block": {
      "index": 2,
      "previousHash": "e95d2bf9976df2f5b40675bedc1c3077a058f6edd3e1631efcec7650de53b551",
      "data": [
          {
              "type": "coinbase",
              "txIns": [
                  {
                      "txOutIndex": 2
                  }
              ],
              "txOuts": [
                  {
                      "address": "",
                      "amount": 0
                  }
              ],
              "timestamp": 1553787151,
              "id": "d02aa933e10c49849a251c7250d1b66ac058bf2ec5f9969621b07537f754aae3"
          }
      ],
      "beaconIndex": 305426,
      "beaconValue": "D9680BB9A47FB25E28801E7CB826B2D2ECCB1A627097D35AB9168809E514B6B2CAC0CA6F3C2B834086963FDF6C42FF62576B9C0A34F01903F0334C905AD6C98D",
      "timestamp": 1553787151,
      "hash": "4a02fa6c7f243af4bd07de58c2f80b91437ea07f9ac90b03c68f24e9ba93025a"
}
```

## Get balance

#### GET http://127.0.0.1:3001/address/:address/balance

#### About

Get balance of requested address

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| address | String | Yes | Wallet address (public key) |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| balance | Int | Wallet balance |

#### Example

Get balance of requested address

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/address/0231272fba0fb2a54be85ff7b45hy7712d32134338dbbb10e4e3b9272c2a678238/balance -H 'Content-Type: application/json'
```

#### Output:

```
{
    "balance": 100
}
```

## Get unspents

#### GET http://127.0.0.1:3001/address/:address/unspent

#### About

Get inspents of requested address

#### Input (params)

| Filed | Type | Mandatory | Description |
|-------|------|-----------|-------------|
| address | String | Yes | Wallet address (public key) |

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| unspentTxOuts | Array[unspentTxOut] | Array of unspent txOuts |

#### Example

Get unspent txOuts of requested address

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/address/0231272fba0fb2a54be85ff7b45hy7712d32134338dbbb10e4e3b9272c2a678238/unspent -H 'Content-Type: application/json'
```

#### Output:

```
{
    "unspentTxOuts": [
      {
        "txOutIndex": 0,
        "txOutId": "7fe39d6552ea208ce1dae135e5b442a5feb81642c92d4655adf40d2292edd6e0",
        "amount": 100,
        "address": "0231272fba0fb2a54be85ff7b45hy7712d32134338dbbb10e4e3b9272c2a678238"
      }
    ]
}
```

## Get statistic

#### GET http://127.0.0.1:3001/statistic

#### About

Get info about last mined block, total amount of mined transactions and total amount of money transfered via blockchain network

#### Input (params)

Empty

#### Output

| Filed | Type | Description |
|-------|------|-------------|
| lastBlockIndex | Int | Index of last mined block |
| transactionCount | Int | Total amount of mined transactions |
| totalMoneyTransferred | Int | Total amount of money transfered |

#### Example

Get statistic

#### Input:

```
curl -s -X GET http://127.0.0.1:3001/statistic -H 'Content-Type: application/json'
```

#### Output:

```
{
    "lastBlockIndex": 4356,
    "transactionCount": 1004,
    "totalMoneyTransferred": 1300020000
}
```
