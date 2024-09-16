# Testnet

Razor network is a proof of stake network. In order to participate in the network as a validator, you will need to "Stake" your RAZORs. RAZOR tokens are the native tokens in the network and they are compatible with the ERC20 tokens standard.

## Get tokens {#get-tokens}

You will need some Skale Testnet Tokens to pay for transaction fees.
You can get testnet ETH tokens from here:
https://faucet.skale.network/

- Skale Endpoint: https://testnet.skalenodes.com/v1/juicy-low-small-testnet
- Account: address which should receive the testnet tokens.

In order to get started, you will also need some RAZOR tokens on Skale Testnet chain. Drop a message in our [Discord server](https://discord.com/invite/Js4pBny2rw) for tokens.

## Add Skale Testnet network to metamask

1. Use an ethereum compatible browser (e.g. Chrome browser with Metamask plugin)
2. In metamask, click on top right account icon > Settings > Add Network.
3. Fill in the following details:

   | Particulars        | Value                                                                   |
   | ------------------ | ----------------------------------------------------------------------- |
   | Network Name       | juicy-low-small-testnet                                                 |
   | New RPC URL        | https://testnet.skalenodes.com/v1/juicy-low-small-testnet               |
   | Chain ID           | 0x561bf78b                                                              |
   | Currency Symbol    | ETH                                                                     |
   | Block Explorer URL | https://juicy-low-small-testnet.explorer.testnet.skalenodes.com/        |

   > **Note**: _You can also add network from https://staging.razorscan.io/ by clicking on "Connect wallet" and switching network to Skale._

Now you are all set! Let's download the client and start staking!

## Using Docker {#using-docker}

It is recommended to run a **Oracle Node** using **Docker**. This is because you dont need a complete development enviroment to run a node. Since code is updated and deployed frequently from our github repository, we keep the Oracle Node docker image updated.

## Hardware Requirements {#hardware-requirements}

16GB RAM

4 Core (arm64 or amd64 architecture)

## Software dependencies {#software-dependencies}

Docker: You can find more information about installing docker [here](https://docs.docker.com/engine/install/).

Oracle-Node(github): You can check the Razor-go:dev at [develop branch](https://github.com/razor-network/oracle-node/tree/develop).

You can download the docker image of Razor-go:dev - `54371f1` from [here]https://hub.docker.com/layers/razornetwork/razor-go/54371f1/images/sha256-30770a4b5b427ea1e9542842de208f08df76668923465d61463e8be3589d53b6?context=explore.

### Run the Razor Network Docker Node {#run-the-razor-network-docker-node}

One of the quickest ways to get `razor-go` up and running on your machine is by using Docker:

1. Create docker network

```
docker network create razor_network
```

2. Start razor-go container

```
docker run -d -it --entrypoint /bin/sh --network=razor_network --name razor-go -v "$(echo $HOME)"/.razor:/root/.razor razornetwork/razor-go:ab83868
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
docker exec -it razor-go razor setConfig --provider https://testnet.skalenodes.com/v1/juicy-low-small-testnet --gasmultiplier 1 --buffer 5 --wait 1 --gasprice 0 --logLevel debug --gasLimit 2 --rpcTimeout 10 --httpTimeout 10 --logFileMaxSize 200 --logFileMaxBackups 10 --logFileMaxAge 60
```

> **Note**: _This will create `razor.yaml` with all necessary parameter at `$HOME/.razor` directory. We can view that via command:`cat $HOME/.razor/razor.yaml` ._

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

That's it! You should have a staker up and running. Your node will start automatically fetching and answering queries. You must keep your computer online to be able to validate without any interruptions. You can monitor the logs, and use [Staging Razorscan](https://staging.razorscan.io) to monitor your staker.

For more details around all the commands of `razor-go`, please check out the `oracle-node` [Readme](https://github.com/razor-network/oracle-node#readme).

## Installation From Source {#installation-from-source}

If you would rather install from source, please follow Instructions here to [run a Razor Network node from source](https://github.com/razor-network/oracle-node#building-the-source).
