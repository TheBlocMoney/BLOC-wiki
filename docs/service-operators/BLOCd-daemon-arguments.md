# **BLOCd Command Line Arguments**

This section describes BLOCd daemon starting process with correct arguments enabling access to the BLOCd [Command Line Interface](BLOCd-daemon-cli-options.md) , the [HTTP RPC API](BLOCd-daemon-http-rpc-api.md) and/or the [JSON RPC API](BLOCd-daemon-json-rpc-api.md).
 
[BLOCd](BLOCd-Overview.md) can accept settings through a configuration file and/or command line.
 
Almost all of the command line options can be defined through the configuration file. If a parameter is defined in the config and was also indicated in the command line, two behaviors are possible:
 
- If the parameter accepts one value only (e.g., rpc-bind-ip), the command line value will be used, since it has a higher priority
- If the parameter accepts several values (e.g., add-priority-node), then command line and configuration file values will be merged
- If some of the options are not defined in the config, the default values will be applied
- By default config file's name is BLOC.conf, which is located in the binaries folder.
- You may adjust the destination to the file via `--config-file` option in the command line.
 
 Also:

- Config files, where used, now use **JSON** formatted files instead of INI
- Config files will be automatically upgraded to JSON and overwritten on first use

## **BLOC-DEVELOPER**

This page is only a short guide how to get you started with BLOCd configuration. Please visit the [dedicated section on the BLOC-DEVELOPER](https://bloc-developer.com/api_BLOCd/cli_arguments) website to view and test all the features available from the [BLOCd Daemon](BLOCd-Overview.md) Command Line Arguments.

## **Screenshot**

![BLOCd MAIN NET](images/BLOCd-MAIN-NET-v3.0.1.png)

The following examples are made using a Linux system but the concept is the same for all the OS supported by the **BLOCd**.

## **Core Options**

```
Usage:
  ./BLOCd [OPTION...]

 Core options:
      --help                                       Display this help message
      --os-version                                 Output Operating System version information
      --version                                    Output daemon version information
```

### --help

Display the help message and configuration settings.

#### Example

```
./BLOCd --help
```

##### Expected results

![--help](images/BLOCd/arguments/BLOCd-Arguments-help.png)


### --version

Output daemon version information

#### Example

```
./BLOCd --version
```

##### Expected results

![--help](images/BLOCd/arguments/version.png)


### --os-version

Output Operating System version information

#### Example

```
./BLOCd --os-version
```

##### Expected results

![--help](images/BLOCd/arguments/os-version.png)


## **Daemon Options**

```
-c, --config-file <path>                     Specify the <path> to a configuration file
--data-dir <path>                            Specify the <path> to the Blockchain data directory (default: /home/test/.BLOC)
--dump-config                                Prints the current configuration to the screen
--load-checkpoints <path>                    Specify a file <path> containing a CSV of Blockchain checkpoints for faster sync. A value of 'default' uses the built-in checkpoints. (default: default)
--log-file <path>                            Specify the <path> to the log file (default: ./BLOCd.log)
--log-level #                                Specify log level (default: 2)
--no-console                                 Disable daemon console commands
--save-config <file>                         Save the configuration to the specified <file>

```

### --config-file arg (=BLOC.conf)

Specify a configuration file to start BLOCd. This is much more simple to use if you have a particular configuration and you do not want to type all the arguments while launching BLOCd.

#### Example

```
./BLOCd --config-file=BLOC.conf
```

##### Expected results

![--config-file](images/BLOCd/arguments/CONF.png)


### --data-dir arg (=/home/bloc/.BLOC)

* Specify another data directory than the original one set by BLOCd.
* The data directory contains the blockchain files from BLOC.
* Creating a new empty data directory will resynch the blockchain from 0.
* (default:/home/test/.BLOC)

#### Example

```
./BLOCd --data-dir=/home/bloc/.MYFOLDER
```

##### Expected results

![--help](images/BLOCd/arguments/data-dir.png)


### --dump-config

Prints the current configuration of BLOCd to the screen.

#### Example

```
./BLOCd --dump-config
```

##### Expected results

![--config-file](images/BLOCd/arguments/dump-config.png)


### Create .CONF file

1. Use the command line argument `--save-config` while starting BLOCd. Copy and paste this to a new file.
2. Check all your required parameters and enter them like in this example
3. You can use the [BLOC-DEVELOPER BLOCd Argument](https://bloc-developer.com/api_BLOCd/cli_arguments) online tool creator
4. You need to type the arguments without the '--'
5. Place this file next to BLOCd
6. Start BLOCd like this: `./BLOCd --config-file=BLOC.conf`

#### Example

```
{
  "add-exclusive-node": [],
  "add-peer": [],
  "add-priority-node": [],
  "allow-local-ip": false,
  "data-dir": "/home/test/.BLOC",
  "db-max-open-files": 100,
  "db-read-buffer-size": 10,
  "db-threads": 2,
  "db-write-buffer-size": 256,
  "enable-blockexplorer": false,
  "enable-cors": [],
  "fee-address": "",
  "fee-amount": 0,
  "hide-my-port": false,
  "load-checkpoints": "default",
  "log-file": "./BLOCd.log",
  "log-level": 2,
  "no-console": false,
  "p2p-bind-ip": "0.0.0.0",
  "p2p-bind-port": 2082,
  "p2p-external-port": 0,
  "rpc-bind-ip": "127.0.0.1",
  "rpc-bind-port": 2086,
  "seed-node": []
}
```
[Download Example](images/BLOCd/arguments/BLOC.conf)


### load-checkpoints arg (=default) <default|filename>

* Use builtin default checkpoints
* Or use checkpoint csv file for faster initial blockchain sync
* Specify a file `<path>` containing a CSV of Blockchain checkpoints for faster sync.
* A value of `default` uses the built-in checkpoints.

#### Example

```
./BLOCd --load-checkpoints=checkpoints.csv
```

##### Expected results

[Cick here](../using-check-points-with-BLOCd.md).


**Possible Errors**
```
ERROR Exception: Directory does not exist: /home/bloc/.MYFOLDER
```
Remark: Make sure you have created the folder you want to use before start BLOCd


### --log-file arg (=test.log)

* Specify another log file than the original one created by BLOCd named (BLOCd.log)
* The specified log file will be created in the same folder where BLOCd was started

#### Example

```
./BLOCd --log-file=test.log
```

##### Expected results

![--help](images/BLOCd/arguments/log-file.png)

File created next to BLOCd:

![--help](images/BLOCd/arguments/testlog.png)


### --log-level arg (=2)

* Specify another log file than the original one created by BLOCd with a level 2
* There is 5 different level. The higher you choose, the more details you get.
* Log level must be 0...5

#### Example

```
./BLOCd --log-level=2
```

##### Expected results

![--help](images/BLOCd/arguments/log-level.png)


### --no-console

* Disable the BLOCd daemon console.
* Can be usefull in case you do not want to allow anyone to run commands through the BLOCd daemon
* As you can see on the screenshot, nothing happen when running the 'help' command

#### Example

```
./BLOCd --no-console
```

##### Expected results

![--help](images/BLOCd/arguments/no-console.png)


### --save-config

Save the configuration to the specified `<file>`

#### Example

```
./BLOCd --save-config myconfig.conf
```

##### Expected results

![--help](images/BLOCd/arguments/save-config.png)


## **Database Options**

```
--db-max-open-files #        Number of files that can be used by the database at one time (default: 100)
--db-read-buffer-size #      Size of the database read cache in megabytes (MB) (default: 10)
--db-threads #               Number of background threads used for compaction and flush operations (default: 2)
--db-write-buffer-size #     Size of the database write buffer in megabytes (MB) (default: 256)
```

### --db-max-open-files arg (=100)

* Number of files that can be used by the database at one time
* (default: 100)

#### Example

```
./BLOCd --db-max-open-files=100
```

### --db-read-buffer-size arg (=10)

* Size of the database read cache in megabytes (MB)
* (default: 10)

#### Example

```
./BLOCd --db-read-buffer-size=10
```

### --db-write-buffer-size arg (=256)

* Size of the database write buffer in megabytes (MB)
* (default: 256)

#### Example

```
./BLOCd --db-write-buffer-size=256
```

### --db-threads arg (=2)

* Number of background threads used for compaction and flush
* Default is 2

#### Example

```
./BLOCd --db-threads=2
```


## Genesis Block Options

Fork [BLOC](https://github.com/furiousteam/BLOC). and create your your own cryptocurrency.

```
--genesis-block-reward-address <address>     Specify the address for any premine genesis block rewards
--print-genesis-tx                           Print the genesis block transaction hex and exits
```

### --genesis-block-reward-address arg

* Premine wallet address
* Use this method combined with the next one if you like to generate a premine wallet into your new created cryptocurrency
* More details can be found in the CryptoNoteConfig.h

#### Example

```
./BLOCd --print-genesis-tx --genesis-block-reward-address=abLoc9fgn3Lcirw7U6nthwTBgwoffUJajEHr3vtSb9nPPL91XWG1Brt5TNCKRZojEbCGhMdSSjpCQfiMnfGEzMQbfs25N6HC6JR
```

##### Expected results

![--help](images/BLOCd/arguments/genesis-block-reward-address.png)


### --print-genesis-tx

* Prints genesis' block tx hex to insert it to config and exits
* Use this method if you like to fork BLOC to generate a new 'GENESIS_COINBASE_TX_HEX' and create your own cryptocurrency
* More details can be found in the CryptoNoteConfig.h

#### Example

```
./BLOCd --print-genesis-tx
```

##### Expected results

![--help](images/BLOCd/arguments/print-genesis-tx.png)


## **Network Options**

```
--allow-local-ip                             Allow the local IP to be added to the peer list
--hide-my-port                               Do not announce yourself as a peerlist candidate
--p2p-bind-ip <ip>                           Interface IP address for the P2P service (default: 0.0.0.0)
--p2p-bind-port #                            TCP port for the P2P service (default: 2082)
--p2p-external-port #                        External TCP port for the P2P service (NAT port forward) (default: 0)
--rpc-bind-ip <ip>                           Interface IP address for the RPC service (default: 127.0.0.1)
--rpc-bind-port #                            TCP port for the RPC service (default: 2086)
```

### --allow-local-ip

* Allow the local IP to be added to the peer list, mostly in debug purposes

#### Example

```
./BLOCd --allow-local-ip
```

##### Expected results

![--help](images/BLOCd/arguments/allow-local-ip.png)


### --hide-my-port

* Do not announce yourself as a peerlist candidate

#### Example

```
./BLOCd --hide-my-port
```

### --p2p-bind-ip arg (=0.0.0.0) 

* Interface for p2p network protocol
* Started by default on 0.0.0.0 when running ./BLOCd
* If you want to use local only : 127.0.0.1
* if you want to open to public : 0.0.0.0
* (default: 0.0.0.0)

#### Example

```
(Public)
./BLOCd --p2p-bind-ip=0.0.0.0

(Local)
./BLOCd --p2p-bind-ip=127.0.0.1
```
##### Expected results

![--help](images/BLOCd/arguments/p2p-bind-ip.png)


### --p2p-bind-port arg (=2082)

* Port for p2p network protocol
* Started by default on 2082 when running ./BLOCd
* You can change this port here

#### Example

```
./BLOCd --p2p-bind-port=3000
```

##### Expected results

![--help](images/BLOCd/arguments/p2p-bind-port.png)


### --p2p-external-port arg (=0)

* External TCP port for the P2P service (NAT port forward)
* (default: 0)

#### Example

```
./BLOCd --p2p-external-port=5000
```

##### Expected results

![--help](images/BLOCd/arguments/p2p-external-port.png)


### --rpc-bind-ip arg (=127.0.0.1)

* Interface for RPC service
* Started by default on 127.0.0.1 when running ./BLOCd
* If you want to use local only : 127.0.0.1
* if you want to open to public : 0.0.0.0
* More details about the [HTTP RPC API](../BLOCd-daemon-http-rpc-api.md)
* More details about the [JSON RPC API](../BLOCd-daemon-json-rpc-api.md)

#### Example

```
(Public)
./BLOCd --rpc-bind-ip=0.0.0.0

(Local)
./BLOCd --rpc-bind-ip=127.0.0.1
```

##### Expected results

![--help](images/BLOCd/arguments/rpc-bind-ip.png)


### --rpc-bind-port arg (=2086)

* Port for the RPC service
* Started by default on 2086 when running ./BLOCd
* You can change this port here
* More details about the [HTTP RPC API](../BLOCd-daemon-http-rpc-api.md)
* More details about the [JSON RPC API](../BLOCd-daemon-json-rpc-api.md)

#### Example

```
./BLOCd --rpc-bind-port=10000
```

##### Expected results

![--help](images/BLOCd/arguments/rpc-bind-port.png)


## **Peer Options**

```
--add-exclusive-node <ip:port>               Manually add a peer to the local peer list ONLY attempt connections to it. [ip:port]
--add-peer <ip:port>                         Manually add a peer to the local peer list
--add-priority-node <ip:port>                Manually add a peer to the local peer list and attempt to maintain a connection to it [ip:port]
--seed-node <ip:port>                        Connect to a node to retrieve the peer list and then disconnect
```

### --add-exclusive-node arg

* Manually add a peer to the local peer list ONLY attempt connections to it. [ip:port]
* If this option is given the options add-priority-node and seed-node are ignored

#### Example

```
./BLOCd --add-exclusive-node=NODE.IP.ADDRESS
```

### --add-peer arg

* Manually add a peer to the local peer list

#### Example

```
./BLOCd --add-peer=PEER.IP.ADDRESS
```

### --add-priority-node arg

* Manually add a peer to the local peer list and attempt to maintain a connection to it [ip:port]

#### Example

```
./BLOCd --add-priority-node=NODE.IP.ADDRESS
```

### --seed-node arg

* Connect to a node to retrieve the peer list and then disconnect

#### Example

```
./BLOCd --seed-node=NODE.IP.ADDRESS
```

## **RPC Options**

```
--enable-blockexplorer                       Enable the Blockchain Explorer RPC
--enable-cors <domain>                       Adds header 'Access-Control-Allow-Origin' to the RPC responses using the <domain>. Uses the value specified as the domain. Use * for all.
--fee-address <address>                      Sets the convenience charge <address> for light wallets that use the daemon
--fee-amount #                               Sets the convenience charge amount for light wallets that use the daemon (default: 0)
```

### --enable-blockexplorer

* To enable block explorer API access (like for getblocks, gettransactionpool, etc.)

#### Example

```
./BLOCd --enable-blockexplorer
```

##### Expected results

![--help](images/BLOCd/arguments/enable-bloc-explorer.png)


### --enable-cors arg

* Adds header 'Access-Control-Allow-Origin' to the RPC responses using the `<domain>`
* Uses the value specified as the domain
* Use `*` for all.

#### Example

```
./BLOCd --enable-cors=*
./BLOCd --enable-cors=yourdomain.com
```

##### Expected results

![--help](images/BLOCd/arguments/enable-cors.png)

##### Response Headers
```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 123
Content-Type: application/json
Server: CryptoNote-based HTTP server

or

HTTP/1.1 200 OK
Access-Control-Allow-Origin: yourdomain.com
Content-Length: 123
Content-Type: application/json
Server: CryptoNote-based HTTP server
```

### --fee-address arg

* This is a new feature implemented in BLOC v3.0
* Read more about [Nodes Fees](../wallets/Using-remote-nodes.md).
* Sets the convenience charge `<address>` for light wallets that use the daemon
* Make sure you combine this argument with `--fee-amount`

#### Example

```
./BLOCd --fee-address=abLoc9fgn3Lcirw7U6nthwTBgwoffUJajEHr3vtSb9nPPL91XWG1Brt5TNCKRZojEbCGhMdSSjpCQfiMnfGEzMQbfs25N6HC6JR
```

### --fee-amount arg (=0)

* This is a new feature implemented in BLOC v3.0
* Read more about [Nodes Fees](../wallets/Using-remote-nodes.md).
* Sets the convenience charge amount for light wallets that use the daemon (default: 0)
* Make sure you combine this argument with `--fee-address are`
* Remember BLOC is using Atomic Units
* `--fee-amount=1` means 0.0001 BLOC fees will be sent to the `--fee-address` specified
* `--fee-amount=1000` means 1 BLOC fees will be sent to the `--fee-address` specified

#### Example

```
./BLOCd --fee-address=abLoc9fgn3Lcirw7U6nthwTBgwoffUJajEHr3vtSb9nPPL91XWG1Brt5TNCKRZojEbCGhMdSSjpCQfiMnfGEzMQbfs25N6HC6JR --fee-amount=1
```

##### Expected results

![--help](images/BLOCd/arguments/fee-amount.png)


## BLOC-DEVELOPER

Make sure you visit the dedicated website [BLOC-DEVELOPER.com](https://bloc-developer.com) to find out more details and test your application.

If anything is missing or seems incorrect, please check the [GitHub issues](https://github.com/furiousteam/BLOC-wiki/issues) for existing known issues or [create a new one](https://github.com/furiousteam/BLOC-wiki/issues/new).