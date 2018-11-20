# Wallet RPC API

[BLOC](https://bloc.money) RPC Wallet is a HTTP server which provides JSON 2.0 RPC interface for **BLOC**** payment operations and address management.

## Errors

* Make sure you check the [RPC Errors conditions](../API/rpc-api-error-conditions.md).

Currently we support the following official client bindings:

* [JavaScript](https://www.npmjs.com/package/bloc-rpc)
* [PHP](https://github.com/furiousteam/BLOC-rpc-php)
* [Go](https://github.com/furiousteam/BLOC-rpc-go)

```javascript
npm install bloc-rpc
```

```php
composer require furiousteam/BLOC-rpc-php
```

```go
go get github.com/furiousteam/BLOC-rpc-go
```

## Interacting with the API

> API endpoint example

```
http://localhost:8070/json_rpc
```

> Configuration and instantiation

```javascript
const BlocService = require('bloc-rpc').BlocService

const service = new BlocService({
  host: '127.0.0.1', // ip address or hostname of the bloc-service host
  port: 8070, // what port is bloc-service running on
  timeout: 2000, // request timeout
  ssl: false, // whether we need to connect using SSL/TLS
  rpcPassword: 'inblocwetrust', // must be set to the password used to run bloc-service

  // RPC API default values
  defaultMixin: false, // the default mixin to use for transactions, the default setting is false which means we don't have a default value
  defaultFee: 1, // the default transaction fee for transactions
  defaultBlockCount: 1, // the default number of blocks when blockCount is required
  decimalDivisor: 1000, // Currency has many decimal places?
  defaultFirstBlockIndex: 1, // the default first block index we will use when it is required
  defaultUnlockTime: 0, // the default unlockTime for transactions
  defaultFusionThreshold: 1, // the default fusionThreshold for fusion transactions
})
```

```php
<?php
use BLOC\BlocService;

$config = [
    'rpcHost'     => 'http://localhost',
    'rpcPort'     => 8070,
    'rpcPassword' => 'passw0rd',
];

$blocService = new BlocService($config);
```

```go
import (
  "fmt"
  trpc "github.com/furiousteam/BLOC-rpc-go"
)

rpcHost := "localhost"
rpcPort := 8070
rpcPassword := "passw0rd"

service := trpc.Walletd{
  URL: rpcHost,
  Port: rpcPort,
  RPCPassword: rpcPassword}
```

To make a JSON RPC request to your TurtleCoin RPC Wallet you should use a POST request that looks like this:

`http://<service address>:<service port>/json_rpc`

Parameter            | Description
-------------------- | ------------------------------------------------------------
`<service address>`  | IP of TurtleCoin RPC Wallet, if RPC Wallet is located on local machine it is either 127.0.0.1 or localhost
`<service port>`     | TurtleCoin RPC Wallet port, by default it is bound to 8070 port, but it can be manually bound to any port you want



## reset

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"reset","params":{"viewSecretKey":"xxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
service.reset({
  viewSecretKey: '12345678901234567890'
}).then(() => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$viewSecretKey = 'xxxxx...';
$response = $blocService->reset($viewSecretKey);
echo $response;
```

```go
scanHeight := 0 // starting height to scan
response, err := service.Reset(scanHeight)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`reset()` method allows you to re-sync your wallet.

**Input**

Argument         | Mandatory   | Description      | Format
---------------- | ----------- | ---------------- | ------
viewSecretKey    | No          | Private view key | string
newAddress       | No          | Is this a new address being created? If so, blocks before the creation timestamp will not be scanned. Only one of newAddress and scanHeight can be specified, as if a new address is being created, there is no need to scan from a certain height. | bool
scanHeight       | No          | The height to begin scanning for transactions at. This can greatly speed up wallet syncing time. | int



No output in case of success.

<aside class="notice">
  <div>If the <code>viewSecretKey</code> argument is not provided, the <code>reset()</code> method resets the wallet and
  re-syncs it. If the <code>viewSecretKey</code> argument is provided, the <code>reset()</code> method substitutes the
  existing wallet with a new one with the specified key.</div>
</aside>



## save

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"save","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.save().then(() => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$response = $blocService->save();
echo $response;
```

```go
response, err := service.Save()
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`save()` method allows you to save your wallet by request.

No input.
No output in case of success.



## getViewKey

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getViewKey","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getViewKey().then((result) => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$response = $blocService->getViewKey();
echo $response;
```

```go
response, err := service.GetViewKey()
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "viewSecretKey":"xxxxx..."
  }
}
```

`getViewKey()` method returns your view key.

No input.

**Output**

Argument         | Description      | Format
---------------- | ---------------- | ------
viewSecretKey    | Private view key | string



## getSpendKeys

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getSpendKeys","params":{"address":"abLocxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
service.getSpendKeys({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$address = 'abLocxxxx...';
$response = $blocService->getSpendKeys($address);
echo $response;
```

```go
address := "abLocxxxx..."
response, err := service.GetSpendKeys(address)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "spendSecretKey":"xxxxx...",
    "spendPublicKey":"xxxxx..."
  }
}
```

`getSpendKeys()` method returns your spend keys.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | Valid address that exists in this container  | string

**Output**

Argument          | Description          | Format
----------------  | -------------------- | ------
spendSecretKey    | Private spend key    | string
spendPublicKey    | Public spend key     | string



## getMnemonicSeed

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getMnemonicSeed","params":{"address":"abLocxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
service.getMnemonicSeed({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$address = 'abLocxxxx...';
$response = $blocService->getMnemonicSeed($address);
echo $response;
```

```go
address := "abLocxxxx..."
response, err := service.GetMnemonicSeed(address)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "mnemonicSeed":"turtles are cool..."
  }
}
```

`getMnemonicSeed()` method returns the mnemonic seed for the given _deterministic_ address. A mnemonic seed is a list of words which can be used to recover a wallet.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | Valid deterministic address that exists in this container | string

**Output**

Argument          | Description          | Format
----------------  | -------------------- | ------
mnemonicSeed      | Mnemonic seed        | string

<aside class="notice">
  <div>The first wallet address that is generated when the container is created is the deterministic address. Only one wallet from a multi-wallet container can be deterministic. If a non-deterministic address is given, the RPC response will be an error with the message: "Keys not deterministic."</div>
</aside>



## getStatus

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getStatus","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getStatus().then((result) => {
  // do something
}).catch((error) => {
  // do something
})
```

```php
<?php
$response = $blocService->getStatus();
echo $response;
```

```go
response, err := service.GetStatus()
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "blockCount":455956,
    "knownBlockCount":455955,
    "lastBlockHash":"8d6f8...",
    "peerCount":8
  }
}
```

`getStatus()` method returns information about the current RPC Wallet state: block count, known block count, last block hash and peer count.

No input.

**Output**

Argument         | Description                                                                | Format
---------------- | -------------------------------------------------------------------------- | ------
blockCount       | Node's known number of blocks                                              | int
knownBlockCount  | Maximum known number of blocks of all seeds that are connected to the node | int
lastBlockHash    | Hash of the last known block                                               | string
peerCount        | Connected peers number	                                                  | int	 



## getAddresses

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getAddresses","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getAddresses().then((result) => {
  // do something
}).catch((error) => {
  //do something
})
```

```php
<?php
$response = $blocService->getAddresses();
echo $response;
```

```go
response, err := service.GetAddresses()
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "addresses":[
      "abLocxxxx...",
      "abLocxxxx..."
    ]
  }
}
```

`getAddresses()` method returns an array of your RPC Wallet's addresses.

No input.

**Output**

Argument          | Description                                           | Format
----------------- | ----------------------------------------------------- | ------
addresses	      | Array of strings, where each string is an address	  | array



## createAddress

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"createAddress","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.createAddress({
  secretSpendKey: '',
  publicSpendKey: ''
}).then((result) => {
  // do something
}).catch((error) => {
  //do something
})
```

```php
<?php
$secretSpendKey = null;
$publicSpendKey = null;
$response = $blocService->createAddress($secretSpendKey, $publicSpendKey);
echo $response;
```

```go
spendSecretKey := ""
spendPublicKey := ""
scanHeight := 850000
newAddress := true
response, err := service.CreateAddress(spendSecretKey, spendPublicKey, scanHeight, newAddress)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "address":"abLocxxxx..."
  }
}
```

`createAddress()` method creates an additional address in your wallet.

**Input**

Argument                 | Mandatory    | Description                                  | Format
------------------------ | ------------ | -------------------------------------------- | -------
secretSpendKey           | No           | Private spend key. If `secretSpendKey` was specified, RPC Wallet creates spend address | string
publicSpendKey           | No           | Public spend key. If `publicSpendKey` was specified, RPC Wallet creates view address   | string
newAddress               | No           | Is this a new address being created? If so, blocks before the creation timestamp will not be scanned. Defaults to true if neither keys are given, as it is guaranteed to be a new address. | bool
scanHeight               | No           | The height to begin scanning for transactions at. Only applies if a public/secret key is supplied. This can greatly speed up wallet syncing time. | int



## deleteAddress

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"deleteAddress","params":{"address":"abLocxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
service.deleteAddress({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
})
```

```php
<?php
$address = 'abLocxxxx...';
$response = $blocService->deleteAddress($address);
echo $response;
```

```go
address := "abLocxxxx..."
response, err := service.DeleteAddress(address)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`deleteAddress()` method deletes a specified address.

**Input**

Argument         | Mandatory    | Description                                  | Format
---------------- | ------------ | -------------------------------------------- | -------
address          | Yes          | An address to be deleted                     | string

**Output**

In case of success returns an empty JSON object.



## getBalance

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getBalance","params":{"address":"abLocxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
// Address optional
service.getBalance({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
})
```

```php
<?php
$address = 'abLocxxxx...';
$response = $blocService->getBalance($address);
echo $response;
```

```go
address := "abLocxxxx..."
response, err := service.GetBalance(address)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "availableBalance":10000,
    "lockedAmount":0
  }
}
```

`getBalance()` method returns a balance for a specified address.

**Input**

Argument         | Mandatory    | Description                                          | Format
---------------- | ------------ | ---------------------------------------------------- | -------
address          | No           | Valid address that exists in this container          | string

**Output**

Argument              | Description                                           | Format
--------------------- | ----------------------------------------------------- | ------
availableBalance      | Available balance of the specified address in shells  | int
lockedAmount          | Locked amount of the specified address in shells      | int

<aside class="notice">
  <div>If address is not specified, <code>getBalance()</code> returns a cumulative balance of all RPC Wallet's addresses.
  Also note, balances are expressed in shells, so a balance of 10000 is equal to 100.00 abLoc.</div>
</aside>



## getBlockHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getBlockHashes","params":{"firstBlockIndex":0,"blockCount":3}}' http://localhost:8070/json_rpc
```

```javascript
service.getBlockHashes({
  firstBlockIndex: 500000,
  blockCount: 10
}).then((result) => {
  // do something
})
```

```php
<?php
$firstBlockIndex = 0;
$blockCount = 3;
$response = $blocService->getBlockHashes($firstBlockIndex, $blockCount);
echo $response;
```

```go
firstBlockIndex := 0
blockCount := 3
response, err := service.GetBlockHashes(firstBlockIndex, blockCount)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "blockHashes":[
      "7fb97...",
      "8c973...",
      "2ef06..."
    ]
  }
}
```

`getBlockHashes()` method returns an array of block hashes for a specified block range.

**Input**

Argument         | Mandatory    | Description                                     | Format
---------------- | ------------ | ----------------------------------------------- | -------
firstBlockIndex  | Yes          | Starting height	                              | int
blockCount       | Yes          | Number of blocks to process		              | int

**Output**

Argument              | Description                                             | Format
--------------------- | ------------------------------------------------------- | ------
blockHashes		      | Array of strings, where each element is a block hash	| array



## getTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransactionHashes","params":{"firstBlockIndex":400000,"blockCount":100000}}' http://localhost:8070/json_rpc
```

```javascript
service.getTransactionHashes({
  addresses: [
    "abLocux9QBmzCYEGgdWXHEQCAm6vY9vZHkbGmx8ev5LxhYk8N71Pp7PWFYL9CHxpWph2wCPZcJ6tkPfUxVZcUN8xmYsSDJZ25i9n",
    "abLocv1mPerM2ckUuNvxrkzDE7QKd9PFVUXYbVfbvx8YxB5BYEdSqQvUFYL9CHxpWph2wCPZcJ6tkPfUxVZcUN8xmYsSDJbQMVgF"
  ],
  blockHash: 'f98d6bbe80a81b3aa0aebd004096e2223524f58f347a1f21be122450f244b948',
  blockCount: 1
}).then((result) => {
  // do something
})
```

```php
<?php
$blockCount = 100000;
$firstBlockIndex = 400000;
$blockHash = null;
$addresses = null;
$paymentId = null;

$response = $blocService->getTransactionHashes(
    $blockCount, $firstBlockIndex, $blockHash, $addresses, $paymentId
);

echo $response;
```

```go
addresses := []string{"abLocxxxx..."}
blockHash := ""
firstBlockIndex := 0
blockCount := 3
paymentID := ""
response, err := service.GetTransactionHashes(addresses, blockHash, firstBlockIndex, blockCount, paymentID)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "items":[
      {
        "blockHash":"f0d8c...",
        "transactionHashes":["529ea..."]
      },
      {
        "blockHash":"4a1ae...",
        "transactionHashes":["2e709..."]
      }
    ]
  }
}
```

`getTransactionHashes()` method returns an array of block and transaction hashes. A transaction consists of transfers.
A transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument         | Mandatory                                                                | Description                                                   | Format
---------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------- | -------
addresses        | No                                                                       | Array of strings, where each string is an address	            | array
blockHash        | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Hash of the starting block                                    | string
firstBlockIndex  | Only one of these parameters (blockHash or firstBlockIndex) is allowed   | Starting height	                                            | int
blockCount       | Yes                                                                      | Number of blocks to return transaction hashes from	        | int
paymentId        | No                                                                       | Valid payment ID	                                            | string

* If `paymentId` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain specified payment ID in the given block range.
* If `addresses` parameter is set, `getTransactionHashes()` method returns transaction hashes of transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactionHashes()` method returns transaction hashes of transactions that contain both specified payment ID and transfer from at least one of specified addresses.

**Output**

Argument   | Description                                         |                                                              |            |                                       
---------- | --------------------------------------------------- | ------------------------------------------------------------ | ---------- |
items	   | **Array of**                                        |	                                                            |            |                                                                 
    	   | **Attribute**            	                         | **Description**                                              | **Format** |                                        
           | blockHash                                           | Hash of the block which contains transaction hashes          | string     |
           | transactionHashes                                   | Array of strings, where each string is a transaction hash    | array      |



## getTransactions

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransactions","params":{"firstBlockIndex":400000,"blockCount":100000}}' http://localhost:8070/json_rpc
```

```javascript
service.getTransactions({
  addresses: [
    "abLocux9QBmzCYEGgdWXHEQCAm6vY9vZHkbGmx8ev5LxhYk8N71Pp7PWFYL9CHxpWph2wCPZcJ6tkPfUxVZcUN8xmYsSDJZ25i9n",
    "abLocv1mPerM2ckUuNvxrkzDE7QKd9PFVUXYbVfbvx8YxB5BYEdSqQvUFYL9CHxpWph2wCPZcJ6tkPfUxVZcUN8xmYsSDJbQMVgF"
  ],
  firstBlockIndex: 469419,
  blockCount: 1
}).then((result) => {
  // do something
})
```

```php
<?php
$blockCount = 100000;
$firstBlockIndex = 400000;
$blockHash = null;
$addresses = null;
$paymentId = null;

$response = $blocService->getTransactions(
    $blockCount, $firstBlockIndex, $blockHash, $addresses, $paymentId
);

echo $response;
```

```go
addresses := []string{"abLocxxxx..."}
blockHash := ""
firstBlockIndex := 0
blockCount := 3
paymentID := ""
response, err := service.GetTransactions(addresses, blockHash, firstBlockIndex, blockCount, paymentID)
if err != nil {
  fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "items":[
      {
        "blockHash":"f0d8c...",
        "transactions":[
          {
            "amount":10000,
            "blockIndex":456018,
            "extra":"01bd0...",
            "fee":10,
            "isBase":false,
            "paymentId":"b6fc6...",
            "state":0,
            "timestamp":1526458339,
            "transactionHash":"529ea...",
            "transfers":[
              {"address":"abLocxxxx...","amount":10000,"type":0},
              {"address":"","amount":-100000,"type":0},
              {"address":"","amount":89990,"type":0}
            ],
            "unlockTime":0
          }
        ]
      },
      {
        "blockHash":"4a1ae...",
        "transactions":[
          {
            "amount":5000,
            "blockIndex":456076,
            "extra":"018c1...",
            "fee":10,
            "isBase":false,
            "paymentId":"55255...",
            "state":0,
            "timestamp":1526460243,
            "transactionHash":"2e709...",
            "transfers":[
              {"address":"abLocxxxx...","amount":5000,"type":0},
              {"address":"","amount":-8000,"type":0},
              {"address":"","amount":2990,"type":0}
            ],
            "unlockTime":0
          }
        ]
      }
    ]
  }
}
```

`getTransactions()` method returns an array of block and transaction hashes.
A transaction consists of transfers. A transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument        | Mandatory                                                                    | Description                                            | Format
--------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------ | -------
addresses       | No                                                                           | Array of strings, where each string is an address		| array
blockHash       | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Hash of the starting block		                        | string
firstBlockIndex | Only one of these parameters (`blockHash` or `firstBlockIndex`) is allowed.  | Starting height >0 (1,2,3...)		                                | int
blockCount      | Yes                                                                          | Number of blocks to return transaction hashes from		| int
paymentId       | No                                                                           | Valid payment ID		                                | string

* If `paymentId` parameter is set, `getTransactions()` method returns transactions that contain specified payment ID in the given block range.
* If `addresses` parameter is set, `getTransactions()` method returns transactions that contain transfer from at least one of specified addresses.
* If both above mentioned parameters are set, `getTransactions()` method returns transactions that contain both specified payment ID and transfer from at least one of specified addresses.

**Output**

Argument   |                              | Description                                       | Format
---------- | ---------------------------- | --------------------------------------------------|-----------
items	   | **Array of**                 |                                                   |
    	   | block_hash                   | hash of the block which contains a transaction    | string
    	   | transactions                 | see below                                         | array

Transaction attributes:

Argument            | Description                                       | Format
------------------- | --------------------------------------------------|-----------
transactionHash     | Hash of the transaction                                                      | string
blockIndex          | Number of the block that contains a transaction                               | int
timestamp           | Timestamp of the transaction                                                  | int
isBase              | Shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | Height of the block when transaction is going to be available for spending    | int
amount              | Amount of the transaction                                                     | int
fee                 | Transaction fee                                                               | int
extra               | Hash of the  transaction                                                      | string
paymentId           | Payment ID of the transaction (optional)                                      | string
transfers           | Array of address (string), amount (int)                                       | array



## getUnconfirmedTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getUnconfirmedTransactionHashes","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getUnconfirmedTransactionHashes({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
})
```

```php
<?php
$addresses = null;
$response = $blocService->getUnconfirmedTransactionHashes($addresses);
echo $response;
```

```go
addresses := []string{"abLocxxxx..."}
response, err := service.GetUnconfirmedTransactionHashes(addresses)
if err != nil {
		fmt.Println(err)
} else {
  fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHashes":[
      "55a23..."
    ]
  }
}
```

`getUnconfirmedTransactionHashes()` method returns information about the current unconfirmed transaction pool or for a specified addresses.

Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument    | Mandatory     | Description                                                | Format
----------- | ------------- | ---------------------------------------------------------- | -------
addresses   | No            | Array of strings, where each string is a valid address     | array

<aside class="notice">
  <div>If addresses parameter is set, transactions that contain transfer from at least one of specified addresses are returned.</div>
</aside>

**Output**

Argument               | Description                                                                    | Format
---------------------- | ------------------------------------------------------------------------------ | ------
transactionHashes      | Array of strings, where each string is a hash of an unconfirmed transaction	| array



## getTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getTransaction","params":{"transactionHash":"55a23..."}}' http://localhost:8070/json_rpc
```

```javascript
service.getTransaction({
  transactionHash: 'd01e448f7b631cebd989e3a150258b0da59c66f96adecec392bbf61814310751'
}).then((result) => {
  // do something
})
```

```php
<?php
$transactionHash = '55a23...';
$response = $blocService->getTransaction($transactionHash);
echo $response;
```

```go
transactionHash := "55a23..."
response, err := service.GetTransaction(transactionHash)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transaction":{
      "amount":5000,
      "blockIndex":456635,
      "extra":"0134b...",
      "fee":10,
      "isBase":false,
      "paymentId":"ac9c5...",
      "state":0,
      "timestamp":1526477499,
      "transactionHash":"55a23...",
      "transfers":[
        {"address":"abLocxxxx...","amount":5000,"type":0},
        {"address":"","amount":-10000,"type":0},
        {"address":"","amount":4990,"type":0}
      ],
      "unlockTime":0
    }
  }
}
```

`getTransaction()` method returns information about a particular transaction.

Transaction consists of transfers. Transfer is an amount-address pair. There could be several transfers in a single transaction.

**Input**

Argument            | Mandatory     | Description                                                | Format
------------------- | ------------- | ---------------------------------------------------------- | -------
transactionHash     | Yes           | Hash of the requested transaction                          | string

**Output**

Argument   | Description
---------- | ------------
transaction| see below

Transaction attributes:

Argument            | Description                                                                   | Format
------------------- | ------------------------------------------------------------------------------|-------
transactionHash     | Hash of the transaction                                                      | string
blockIndex          | Number of the block that contains a transaction                               | int
timestamp           | Timestamp of the transaction                                                  | int
isBase              | Shows if the transaction is a CoinBase transaction or not                     | boolean
unlockTime          | Height of the block when transaction is going to be available for spending    | int
amount              | Amount of the transaction                                                     | int
fee                 | Transaction fee                                                               | int
extra               | Hash of the  transaction                                                      | string
paymentId           | Payment ID of the transaction (optional)                                      | string
transfers           | Array of addresses (string), amount (int)                                  | array



## sendTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendTransaction","params":{"transfers":[{"address":"abLocxxxx...","amount":5000}],"fee":10,"anonymity":3,"changeAddress":"abLocyyyy..."}}' http://localhost:8070/json_rpc
```

```javascript
service.sendTransaction({
  transfers: [
    service.newTransfer('abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ', 1000000)
  ],
  fee: 0.1
}).then((result) => {
  // do something
})
```

```php
<?php
$anonymity = 3;
$fee = 10;
$addresses = null;
$unlockTime = null;
$extra = null;
$paymentId = null;
$changeAddress = 'abLocyyyy...';

$transfers = [
    ["address" => "abLocxxxx...", "amount"  => 5000],
];

$response = $blocService->sendTransaction(
    $anonymity, $transfers, $fee, $addresses, $unlockTime, $extra, $paymentId, $changeAddress
);

echo $response;
```

```go
addresses := []string{"abLocyyyy..."} // can be empty
unlockTime := 0
extra := ""
paymentID := ""
fee := 10
changeAddress := "abLocyyyy..."

transfers := []map[string]interface{}{
  {
    "address" : "abLocxxxx...",
    "amount" : 5000,
  },
}

response, err := service.SendTransaction(addresses, transfers, fee, unlockTime, extra, paymentID, changeAddress)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"ae57e..."
  }
}
```

`sendTransaction()` method allows you to send transaction(s) to one or several addresses. Also, it allows you to use a payment ID for a transaction to a single address.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address to take the funds from                 | array
transfers       | Yes           | Array of objects, address: (string address), amount: (int amount)                        | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is 0.10 abLoc. As with other amounts use whole units, 1 abLoc = 100 units, so 0.1 abLoc = 10 units | int
unlockTime      | No            | The block height at which the transaction will be unlocked for spending.                 | int
anonymity       | Yes           | Privacy (mixin) level from block 800,000 three (3) | int
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | Payment ID                                                                               | string
changeAddress   | No            | Valid and existing address in this container.                                            | string

* If container contains only 1 address, `changeAddress` field can be left empty and the change is going to be sent to this address.
* If addresses field contains only 1 address, `changeAddress` can be left empty and the change is going to be sent to this address.
* In the rest of the cases, `changeAddress` field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## createDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"createDelayedTransaction","params":{"transfers":[{"address":"abLocxxxx...","amount":5000}],"fee":10,"anonymity":3,"changeAddress":"abLocyyyy..."}}' http://localhost:8070/json_rpc
```

```javascript
service.createDelayedTransaction({
  transfers: [
    service.newTransfer('abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ', 1000000)
  ],
  fee: 0.1
}).then((result) => {
  // do something
})
```

```php
<?php
$anonymity = 3;
$fee = 10;
$addresses = null;
$unlockTime = null;
$extra = null;
$paymentId = null;
$changeAddress = 'abLocyyyy...';

$transfers = [
    ["address" => "abLocxxxx...", "amount"  => 5000],
];

$response = $blocService->createDelayedTransaction(
    $anonymity, $transfers, $fee, $addresses, $unlockTime, $extra, $paymentId, $changeAddress
);

echo $response;
```

```go
addresses := []string{"abLocyyyy..."} // can be empty
unlockTime := 0
extra := ""
paymentID := ""
fee := 10
changeAddress := "abLocyyyy..."

transfers := []map[string]interface{}{
  {
    "address" : "abLocxxxx...",
    "amount" : 5000,
  },
}

response, err := service.CreateDelayedTransaction(addresses, transfers, fee, unlockTime, extra, paymentID, changeAddress)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"ae57e..."
  }
}
```

`createDelayedTransaction()` method creates a delayed transaction. Such transactions are not sent into the network automatically and should be pushed using `sendDelayedTransaction` method.

**Input**

Argument        | Mandatory     | Description                                                                              | Format
--------------- | ------------- | ---------------------------------------------------------------------------------------- | -------
addresses       | No            | Array of strings, where each string is an address                                        | array
transfers       | Yes           | Array of address (string), amount (int)                                                  | array
fee             | Yes           | Transaction fee. Minimal fee in TurtleCoin network is 0.10 abLoc. This parameter should be specified in minimal available abLoc units. For example, if your fee is 0.10 abLoc, you should pass it as 10. | int
unlockTime      | No	        | Height of the block until which transaction is going to be locked for spending.	       | int
anonymity       | Yes           | Privacy (mixin) level from block 800,000 three (3) | int
extra           | No            | String of variable length. Can contain A-Z, 0-9 characters.                              | string
paymentId       | No            | Payment ID                                                                               | string
changeAddress   | No            | Valid and existing in this container address.                                            | string

* If container contains only 1 address, `changeAddress` field can be left empty and the change is going to be sent to this address
* If addresses field contains only 1 address, `changeAddress` can be left empty and the change is going to be sent to this address
* In the rest of the cases, `changeAddress` field is mandatory and must contain an address.
* Outputs that were used for this transactions will be locked until the transaction is sent or cancelled

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## getDelayedTransactionHashes

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getDelayedTransactionHashes","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getDelayedTransactionHashes().then((result) => {
  // do something
})
```

```php
<?php
$response = $blocService->getDelayedTransactionHashes();
echo $response;
```

```go
response, err := service.GetDelayedTransactionHashes()
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHashes":["b3e374..."]
  }
}
```

`getDelayedTransactionHashes()` method returns hashes of delayed transactions.

No input.

**Output**

Argument              | Description                                                     | Format
--------------------- | --------------------------------------------------------------- | ------
transactionHashes	  | Array of strings, where each string is a transaction hash		| array



## deleteDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"deleteDelayedTransaction","params":{"transactionHash":"b3e37..."}}' http://localhost:8070/json_rpc
```

```javascript
service.deleteDelayedTransaction({
  transactionHash: 'd01e448f7b631cebd989e3a150258b0da59c66f96adecec392bbf61814310751'
}).then((result) => {
  // do something
})
```

```php
<?php
$transactionHash = 'b3e37...';
$response = $blocService->deleteDelayedTransaction($transactionHash);
echo $response;
```

```go
transactionHash := "50d83..."
response, err := service.DeleteDelayedTransaction(transactionHash)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`deleteDelayedTransaction()` method deletes a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendDelayedTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendDelayedTransaction","params":{"transactionHash":"c37cd..."}}' http://localhost:8070/json_rpc
```

```javascript
service.sendDelayedTransaction({
  transactionHash: 'd01e448f7b631cebd989e3a150258b0da59c66f96adecec392bbf61814310751'
}).then((result) => {
  // do something
})
```

```php
<?php
$transactionHash = 'c37cd...';
$response = $blocService->sendDelayedTransaction($transactionHash);

echo $response;
```

```go
transactionHash := "50d83..."
response, err := service.SendDelayedTransaction(transactionHash)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{}
}
```

`sendDelayedTransaction()` method sends a specified delayed transaction.

**Input**

Argument              | Mandatory      | Description                              | Format
--------------------- | -------------- | ---------------------------------------- | -------
transactionHash       | Yes            | Valid, existing delayed transaction      | string

**Output**

In case of success returns an empty JSON object.



## sendFusionTransaction

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"sendFusionTransaction","params":{"threshold":1000000,"anonymity":0,"addresses":["abLocxxxx...","abLocyyyy..."],"destinationAddress":"abLoczzzz..."}}' http://localhost:8070/json_rpc
```

```javascript
service.sendFusionTransaction({
  destinationAddress: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
}).then((result) => {
  // do something
})
```

```php
<?php
$threshold = 1;
$anonymity = 0;
$addresses = ['abLocxxxx...', 'abLocyyyy...'];
$destinationAddress = 'abLoczzzz...';
$response = $blocService->sendFusionTransaction($threshold, $anonymity, $addresses, $destinationAddress);

echo $response;
```

```go
threshold := 1
addresses := []string{"abLocxxxx...", "abLocyyyy..."}
destinationAddress := "abLoczzzz..."
response, err := service.SendfusionTransaction(threshold, addresses, destinationAddress)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "transactionHash":"93faed..."
  }
}
```

`sendFusionTransaction()` method allows you to send a fusion transaction, by taking funds from selected addresses and
transferring them to the destination address.
If there aren't any outputs that can be optimized, `sendFusionTransaction()` will return an error. You can
use `estimateFusion` to check the outputs, available for the optimization.

**Input**

Argument            | Mandatory  | Description                                                                                          | Format
------------------- | ---------- | ---------------------------------------------------------------------------------------------------- | -------
threshold           | Yes        | Value that determines which outputs will be optimized. Only the outputs, lesser than the threshold value, will be included into a fusion transaction. | int
anonymity           | Yes        | Privacy (mixin) level from block 800,000 three (3)                                                 | int
addresses           | No         | Array of strings, where each string is an address to take the funds from.	                        | array
destinationAddress  | No         | An address that the optimized funds will be sent to. Valid and existing in this container address.	| string

* If container contains only 1 address, `destinationAddress` field can be left empty and the funds are going to be sent to this address.
* If addresses field contains only 1 address, `destinationAddress` can be left empty and the funds are going to be sent to this address.
* In the rest of the cases, `destinationAddress` field is mandatory and must contain an address.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
transactionHash	      | Hash of the sent transaction		| string



## estimateFusion

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"estimateFusion","params":{"threshold":1000000,"addresses":["abLocxxxx...","abLocyyyy..."]}}' http://localhost:8070/json_rpc
```

```javascript
service.estimateFusion({
  threshold: 1,
  addresses:[
    'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ'
  ]
}).then((result) => {
  // do something
})
```

```php
<?php
$threshold = 1000000;
$addresses = ['abLocxxxx...', 'abLocyyyy...'];
$response = $blocService->estimateFusion($threshold, $addresses);

echo $response;
```

```go
threshold := 1000000
addresses := []string{"abLocxxxx...","abLocyyyy..."}
response, err := service.EstimateFusion(threshold, addresses)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id":1,
  "jsonrpc":"2.0",
  "result":{
    "fusionReadyCount":0,
    "totalOutputCount":8
  }
}
```

`estimateFusion()` method counts the number of unspent outputs of the specified addresses and returns how many of those outputs can be optimized.
This method is used to understand if a fusion transaction can be created. If `fusionReadyCount` returns a value = 0, then a fusion transaction cannot be created.

**Input**

Argument            | Mandatory  | Description                                                                                          | Format
------------------- | ---------- | ---------------------------------------------------------------------------------------------------- | -------
threshold           | Yes        | Value that determines which outputs will be optimized. Only the outputs, lesser than the threshold value, will be included into a fusion transaction. | int
addresses           | No         | Array of strings, where each string is an address to take the funds from.	                        | string

**Output**

Argument            | Description                                                 | Format
------------------- | ----------------------------------------------------------- | ------
totalOutputCount	| Total number of unspent outputs of the specified addresses. | int
fusionReadyCount    | Number of outputs that can be optimized.                    | int

## createIntegratedAddress

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"createIntegratedAddress","params":{"paymentId":"7FE73BD90EF05DEA0B5C15FC78696619C50DD5F2BA628F2FD16A2E3445B1922F", "address":"abLocxxxx..."}}' http://localhost:8070/json_rpc
```

```javascript
service.createIntegratedAddress({
  address: 'abLocv1pacKFJk9QgSmzk2LJWn14JGmTKzReFLz1RgY3K9Ryn7783RDT2TretzfYdck5GMCGzXTuwKfePWQYViNs4avKpnUbrwfQ',
  paymentId: '80ec855eef7df4bce718442cabe086f19dfdd0d03907c7768eddb8eca8c5a667'
}).then((result) => {
  // do something
})
```

```php
<?php
$address = 'abLocxxxx...';
$paymentId = '7FE73BD90EF05DEA0B5C15FC78696619C50DD5F2BA628F2FD16A2E3445B1922F';
$response = $blocService->createIntegratedAddress($address, $paymentId);

echo $response;
```

```go
address := "abLocxxxx..."
paymentID := "7FE73BD90EF05DEA0B5C15FC78696619C50DD5F2BA628F2FD16A2E3445B1922F"
response, err := service.CreateIntegratedAddress(address, paymentID)
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "integratedAddress": "abLocxxx..."
  }
}
```

`createIntegratedAddress()` method allows you to create a combined address, containing a standard address and a paymentId, to be used in sendTransaction() or for supplying to a user, instead of using an address and paymentId as separate parameters. This is helpful to ensure users cannot forget to supply a payment Id.

**Input**

Argument              | Mandatory      | Description          | Format
--------------------- | -------------- | -------------------- | -------
address               | Yes            | A valid address      | string
paymentId             | Yes            | A valid paymentId    | string

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
integratedAddress	  | The created integrated address		| string

## getFeeInfo

```shell
curl -d '{"jsonrpc":"2.0","id":1,"password":"passw0rd","method":"getFeeInfo","params":{}}' http://localhost:8070/json_rpc
```

```javascript
service.getFeeInfo().then((result) => {
  // do something
})
```

```php
<?php
$response = $blocService->getFeeInfo();

echo $response;
```

```go
response, err := service.GetFeeInfo()
if err != nil {
	fmt.Println(err)
} else {
	fmt.Println(response)
}
```

> Expected output:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "address": "abLocxxx...",
    "amount": 5000
  }
}
```

`getFeeInfo()` method retrieves the fee and address (if any) that that BLOCd walletd is connecting to is using. This fee will automatically be added to any transactions sent by sendTransaction() or sendDelayedTransaction(). Note it does not apply to sendFusionTransaction().

No input.

**Output**

Argument              | Description                         | Format
--------------------- | ----------------------------------- | ------
address               | The address of the node owner 		| string
amount                | The fee that will be sent to the node owners address with each transaction | int

## License

[![Creative Commons License](/images/cc-by-sa.png)](https://creativecommons.org/licenses/by-sa/3.0/)

The content in this document were originally written by the [Bytecoin (BCN) Developers](https://bytecoin.org/). It is licensed under the [CC BY SA 3.0 license](https://creativecommons.org/licenses/by-sa/3.0/). The source material can be found at the [Bytecoin Wiki](https://wiki.bytecoin.org/).

Also of note, TurtleCoin developers have altered and adapted the content to suit our implementation of the API. This was done independently of the Bytecoin development team. They neither endorse or acknowledge our changes. Feel free to adopt or change our content as per the [CC BY SA 3.0 license](https://creativecommons.org/licenses/by-sa/3.0/) requirements.

_TurtleCoin developers 2018_

Also of note, BLOC developers have altered and adapted the content to suit our implementation of the API. This was done independently of the [TurtleCoin](https://turtlecoin.lol) development team. They neither endorse or acknowledge our changes. Feel free to adopt or change our content as per the [CC BY SA 3.0 license](https://creativecommons.org/licenses/by-sa/3.0/) requirements. 

_BLOC developers 2018_