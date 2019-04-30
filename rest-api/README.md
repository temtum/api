# Dragon API

## Endpoints

##### Get blockchain
```
curl http://localhost:3001/blocks/:page(\d+)?
```
where *page: number* is not mandatory (return first page if no page set)\
Response: *{ blocks: array of objects(Block), pages: number }*

##### Get block by hash
```
curl http://localhost:3001/block/:hash([a-zA-Z0-9]{64})
```
where *hash: string* is a hash of the block\
Response: *block: object(Block)*\
Example:
```
curl http://localhost:3001/block/04bfcab8722991ae774db48f934ca79cfb7dd991229153b9f732ba5334aafcd8e7266e47076996b55a14bf9913ee3145ce0cfc1372ada8ada74bd287450313534b
```

##### Get block by index
```
curl http://localhost:3001/block/:index(\\d+)
```
where *index: number* is an index of the block\
Response: *block: object(Block)*\
Example:
```
curl http://localhost:3001/block/2
```

##### Flexible search method
```
curl -H "Content-type:application/json" --data '{"query" : "2"}' http://localhost:3001/search
```
Provide search in blocks by hash and index and in transactions by hash.\
where *query* can be hash of block or transaction or index of a block(number)\
Response: *block: object(Block)* || *transaction: object(Transaction)*

##### Mine a block (only for dev, production - automine)
```
curl -X POST http://localhost:3001/block/mine
```
Response: *block: object*

##### Create transaction
```
curl -H "Content-type: application/json" --data '{"from": "04b76c594a99455f009f51849ca70e98c96aa8942f32494d53e0312822881ae55839d67a10423fce292af147db622ca42162576caf8f01c24b664de6bce477937b", "to": "0477cab8722991ae774db48f934ca79cfb7dd991229153b9f732ba5334aafcd8e7266e47076996b55a14bf9913ee3145ce0cfc1372ada8ada74bd287450313534b", "amount": 35, "privateKey": "606e5fdc9b4d9019f0f6d73ee9a039a186a9d2dd0589d6e92ce5c45690f578c4"}' http://localhost:3001/transaction/create
```
where *from: string* is address of the output wallet\
where *to: string* is address of the input wallet\
where *amount: number > 1* string is coins' amount\
Response: *transaction: object*

##### Query transaction pool
```
curl http://localhost:3001/transaction/pool
```
Response: *transactionPool: array of objects(Transaction)*

##### Get transaction by id
```
curl http://localhost:3001/transaction/:id([a-zA-Z0-9]{64})
```
where *id: string* (hash of the transaction)\
Response: *transaction: object*\
Example:
```
curl http://localhost:3001/transaction/04bfcab8722991ae774db48f934ca79cfb7dd991229153b9f732ba5334aafcd8e7266e47076996b55a14bf9913ee3145ce0cfc1372ada8ada74bd287450313534b
```

##### Get balance
```
curl http://localhost:3001/address/:address/balance
```
where *address: string* is wallet\
Response: *{balance: balance: number}*\
Example:
```
curl http://localhost:3001/address/04bfcab8722991ae774db48f934ca79cfb7dd991229153b9f732ba5334aafcd8e7266e47076996b55a14bf9913ee3145ce0cfc1372ada8ada74bd287450313534b/balance
```

##### Query information about a specific address (in progress)
```
curl http://localhost:3001/address/04f72a4541275aeb4344a8b049bfe2734b49fe25c08d56918f033507b96a61f9e3c330c4fcd46d0854a712dc878b9c280abe90c788c47497e06df78b25bf60ae64
```

##### Create address

```
curl -X POST http://localhost:3001/address/create
```
Response: *{address: address}*, where *address: string*
