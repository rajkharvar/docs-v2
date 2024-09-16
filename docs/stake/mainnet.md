# Mainnet

Razor network is a proof of stake network. In order to participate in the network as a validator, you will need to "Stake" your RAZORs. RAZOR tokens are the native tokens in the network and they are compatible with the ERC20 tokens standard.

## Get tokens {#get-tokens}

You will need some sFUEL to pay for transaction fees.
You can get sFUEL from here:
https://faucet.razorscan.io/

In order to get started, you will also need some RAZOR tokens on Razor Schain. You can bridge RAZOR tokens from Ethereum to Razor Schain using [Razor Token Bridge](https://bridge.razorscan.io/). More info regarding RAZOR Token bridge can be found [here](/docs/token-bridge/ethereum-razor)

## Add Razor Schain network to metamask

1. Use an ethereum compatible browser (e.g. Chrome browser with Metamask plugin)
2. In metamask, click on top right account icon > Settings > Add Network.
3. Fill in the following details:

   | Particulars        | Value                                                           |
   | ------------------ | --------------------------------------------------------------- |
   | Network Name       | Razor Schain                                                    |
   | New RPC URL        | https://mainnet.skalenodes.com/v1/turbulent-unique-scheat       |
   | Chain ID           | 278611351                                                       |
   | Currency Symbol    | sFUEL                                                           |
   | Block Explorer URL | https://turbulent-unique-scheat.explorer.mainnet.skalenodes.com |

   > **Note**: _You can also add network from https://razorscan.io/ by clicking on "Connect wallet" and switching network to Skale._

Now you are all set! Let's download the client and start staking!

## Using Docker {#using-docker}

It is recommended to run a **Oracle Node** using **Docker**. This is because you don't need a complete development environment to run a node. Since code is updated and deployed frequently from our github repository, we keep the Oracle Node docker image updated.

## Hardware Requirements {#hardware-requirements}

16GB RAM

4 Core (arm64 or amd64 architecture)

## Software dependencies {#software-dependencies}

Docker: You can find more information about installing docker [here](https://docs.docker.com/engine/install/).

Oracle-Node(github): You can download the Oracle-Node:2.0.0 from [here](https://github.com/razor-network/oracle-node/releases/tag/v2.0.0).

You can download the docker image of Razor-go:v2.0.0 from [here](https://hub.docker.com/layers/razornetwork/razor-go/v1.1.0-patch.1/images/sha256-4ae226fb3bcc1a3115a9f747ffd1d89be7fd0eb5e79474892bfaba9bd8d6e730?context=explore).

### Run the Razor Network Docker Node {#run-the-razor-network-docker-node}

One of the quickest ways to get `razor-go` up and running on your machine is by using Docker:

1. Create docker network

```
docker network create razor_network
```

2. Start razor-go container

```
docker run -d -it --entrypoint /bin/sh --network=razor_network --name razor-go -v "$(echo $HOME)"/.razor:/root/.razor razornetwork/razor-go:v2.0.0
```

This spins up a razor-go docker image. You can find all the images on the [Razor Network dockerhub](https://hub.docker.com/u/razornetwork).

## Set Config {#set-config}

There are a set of parameters that are configurable. These include:

- Provider: The RPC URL of the provider you are using to connect to the blockchain.
- Alternate Provider: This is the secondary RPC URL of the provider used to connect to the blockchain if the primary one is not working.
- Gas Multiplier: The value with which the gas price will be multiplied while sending every transaction.
- Buffer Size: Buffer size determines, out of all blocks in a state, in how many blocks the voting or any other operation can be performed.
- Wait Time: This is the number of seconds the system will wait while voting.
- Gas Price: The value of gas price if you want to set manually. If you don't provide any value or simply keep it to 1, the razor client will automatically calculate the optimum gas price and send it.
- Log Level: Normally debug logs are not logged into the log file. But if you want you can set `logLevel` to `debug` and fetch the debug logs.
- Gas Limit: The value with which the gas limit will be multiplied while sending every transaction.
- Gas Limit Override: This value would be used as a gas limit for all the transactions instead of estimating for each transaction.
- RPC Timeout: Number of seconds after which any contract and client calls will time out if it's not responding.
- HTTP Timeout: This is the threshold number of seconds after which an HTTP request for a job will time out.
- Maximum size of log file: This is the maximum size of log file in MB
- Maximum number of backups of log file: This is the maximum number of old log files to retain.
- Maximum age of log file: This is the maximum number of days to retain old log files.

```
docker exec -it razor-go razor setConfig --provider https://mainnet.skalenodes.com/v1/turbulent-unique-scheat --gasmultiplier 1 --buffer 20 --wait 30 --gasprice 0 --logLevel debug --gasLimit 2 --gasLimitOverride 50000000 --rpcTimeout 10 --httpTimeout 10 --logFileMaxSize 200 --logFileMaxBackups 52 --logFileMaxAge 365
```

> **_NOTE:_**: _This will create `razor.yaml` with all necessary parameter at `$HOME/.razor` directory. We can view that via command:`cat $HOME/.razor/razor.yaml` ._

> **_NOTE:_**: _You can add an alternate provider by passing `--alternateProvider [ALTERNATE_PROVIDER]` in the above `setConfig` command which can act as the secondary RPC to your node._

> **_NOTE:_** You can automate all razor-go commands by providing password non-interactively. There are multiple ways to do that
>
> 1.  Provide password via file, All the commands have an additional `--password` flag that you can provide with the file path from which the password must be picked. To run a command with a password flag with the help of docker, the password file should present in `$HOME/.razor/` directory  
>     **Example**: `docker exec -it -d razor-go razor command  --password /root/.razor/<file_name>`
> 2.  Provide password via echo command.  
>     **Example**: `echo "your password" | docker exec -it -d razor-go razor command`  
>     Linux-based system keeps track of the previously executed command., anyone can get the detail of the previous command via the `history` command.  
>     To delete command from history:
>     1.  List the previously executed command: `history` 2. Delete command from history: `history -d <line-number>`

## Commands {#commands}

Run the commands in following way:

    docker exec -it razor-go razor <command>

> **Note**: _It is recomended to use `--logFile <filename>` flag with every razor command this will generate logfile in `.razor` directory which will be helpfull in debuging any issue._

Create an account using the following command:

    docker exec -it razor-go razor create

Import an account using the following command:

    docker exec -it razor-go razor import

Fund this account with sFUEL and RAZOR tokens to start participating in the network.

You can use the full commands (stake) or the short form (s) as shown below.

Start staking using the `addStake` command

    docker exec -it razor-go razor addStake --address <account> --value <value> --logFile <filename>

where `address` is the address that contains RAZOR tokens and `value` is the amount of RAZOR that you want to stake.

An example of this command would be:

    docker exec -it razor-go razor addStake --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --value 10000 --logFile logs

_Note: --weiRazor flag can be passed to provide values in wei_

If you have a 1000.25 RAZOR tokens in your account, you can stake those using the stake command with weiRazor flag.

Example:

```
$ razor addStake --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --value 1000250000000000000000 --weiRazor true
```

If you have a 5678.1001 RAZOR tokens in your account, you can stake those using the stake command with weiRazor flag.

Example:

```
$ ./razor addStake --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --value 5678100100000000000000 --weiRazor true
```

To start accepting delegation, use the delegation command in a new terminal:

    docker exec -it razor-go razor setDelegation --address <address> --status <bool> --commission <commission> --logFile <filename>

where `address` is the address that contains RAZOR tokens, `status` is true or false to turn on or off delegation, and `commission` is the percentage of commission that you can set.

An example of this command would be:

    docker exec -it razor-go razor setDelegation --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --status true --commission 20 --logFile logs

It will enable delegation, and participants can delegate RAZOR tokens to your staker's account.

Start voting using the `vote` command

    docker exec -it razor-go razor vote --address <account> --logFile <filename>

> **Note**: _To run vote command in background you can use `tmux` for that._
>
> 1.  Run: `tmux new -s razor-go`
> 2.  Run vote command
> 3.  To exit from tmux session: press `ctrl+b`, release those keys and press `d`
> 4.  To list your session: `tmux ls`
> 5.  To attach Session back: `tmux attach-session -t razor-go`

An example of this command would be:

    docker exec -it razor-go razor vote --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --logFile logs

If you have delegation enabled and have some commission set, then you can claim that earned commission anytime via the following command:

    docker exec -it razor-go razor claimCommission --address <address> --logFile <filename>

An example of this command would be:

    docker exec -it razor-go razor claimCommission --address 0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c --logFile logs

View Logs

    tail -f $HOME/.razor/logs/[filename].log

An example of this command would be:

    tail -f $HOME/.razor/logs/logs.log

That's it! You should have a staker up and running. Your node will start automatically fetching and answering queries. You must keep your computer online to be able to validate without any interruptions. You can monitor the logs, and use [Razorscan](https://razorscan.io) to monitor your staker.

For more details around all the commands of `razor-go`, please check out the `oracle-node` [Readme](https://github.com/razor-network/oracle-node#readme).

## Installation From Source {#installation-from-source}

If you would rather install from source, please follow Instructions here to [run a Razor Network node from source](https://github.com/razor-network/oracle-node#building-the-source).

## Update docker image

To update the razor-go node version

1. Get the latest docker image from [Docker Hub](https://hub.docker.com/r/razornetwork/razor-go/tags)

> **Note**: -
>
> 1.  _Make sure you don't use *-alpha and *-beta suffixed docker images those are only for internal testing._
> 2.  _Update the node in the Dispute state in order to reduce the chances of an inactivity penalty. The process of updating the node should be completed in less than 5 minutes. Do the following steps as quickly as possible to avoid any inactivity penalties._

2. Check your container is running via `docker ps`, you should get an output like:
   ```
   CONTAINER ID   IMAGE                                  COMMAND     CREATED         STATUS         PORTS     NAMES
   5f0b7d99a71b   razornetwork/razor-go:v1.0.6           "/bin/sh"   3 weeks ago     Up 3 weeks               razor-go
   ```
3. Stop the existing container  
   `docker stop razor-go`
4. Remove the existing container  
   `docker rm razor-go`
5. Run the staker with latest docker image:

   ```
   docker run -d -it --entrypoint /bin/sh --network=razor_network --name razor-go -v "$(echo $HOME)"/.razor:/root/.razor razornetwork/razor-go:<version>
   ```

   example: If latest version is `v2.0.0` then the command would be:

   ```
   docker run -d -it --entrypoint /bin/sh --network=razor_network --name razor-go -v "$(echo $HOME)"/.razor:/root/.razor razornetwork/razor-go:v2.0.0
   ```

6. Check your container is running `docker ps`, you should get an output like:
   ```
   CONTAINER ID   IMAGE                          COMMAND     CREATED          STATUS          PORTS     NAMES
   53ff3ce7c965   razornetwork/razor-go:v2.0.0   "/bin/sh"   17 seconds ago   Up 16 seconds             razor-go
   ```
7. If you want to update your config file, you can run [SetConfig](https://docs.razor.to/docs/stake/mainnet#set-config) command

> **Note**: _If you are running vote command in tmux session_
>
> 1.  Check your razor-go session exists: `tmux ls`
> 2.  Attach existing session: `tmux a -t razor-go`
> 3.  To exit from tmux session: press `ctrl+b`, release those keys and press `d`
> 4.  To list your session: `tmux ls`
> 5.  To attach Session back: `tmux attach-session -t razor-go`

8. Start voting again
   ```
   docker exec -it razor-go razor vote --address <account> --logFile <filename> --gasLimitOverride 50000000
   ```

---
