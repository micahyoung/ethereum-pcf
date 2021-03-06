# Ethereum on Cloud Foundry

Example of private ethereum cluster running on Cloud Foundry

## Overview

The `up.sh` script will stand up 3 apps:

* `bootnodes` - one instance of the service discovery node for the network
* `miners` - one instance of a geth miner node for block creation
* `nodes` - one instance of a geth node to execute transactions

You can access any of these nodes with `cf ssh` and use the `geth` binary at `app/geth` and the data dir at `app/data`. The genesis file and accounts used are provided with the repo and should not be reused in any production environment.

The miner comes up in a stopped state to prevent run-away CPU usage.

## Testing

The `test.sh` script will:

1. Stop the miner if running
1. Mine 1 block to create ETH for account 0
1. Initiate a transfer of ETH from account 0 to account 1
1. Mine 1 block to complete the transfer
1. Show the balances of each account to confirm the transfer has occured 

## Prerequisites

* [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)
* [Docker](https://docker.com/get)
* Account on Cloud Foundry environment with [Container-to-Container networking](https://docs.pivotal.io/pivotalcf/1-10/concepts/understand-cf-networking.html) and [SSH](https://docs.pivotal.io/pivotalcf/1-10/opsguide/config-ssh.html) enabled
  * Recommended: [Pivotal Web Services](https://run.pivotal.io)
  * Supported on [Pivotal Cloud Foundry 1.10](https://docs.pivotal.io/pivotalcf/1-10/pcf-release-notes/index.html) and higher

## Usage

* Install the cf cli network policy plugin
  ```
  cf install-plugin network-policy
  ```

* Log in to Cloud Foundry and target an org and space
  ```
  cf login -a https://api.your-cf.com -u your-email@example.com -o your-org -s your-space
  ```

* Run `up.sh` and wait for it to finish

* Run `test.sh`. Read the prompts and compare it to the output to confirm the cluster is working


## Notes

* Verbosity is turned up on `geth` and `bootnodes` so the logs show lots of false positive errors. Examples:
  ```
  <log stamp> Dial error                               task="dyndial 8ba26367c9651a53 10.253.245.133:33445" err="dial tcp 10.253.245.133:33445: getsockopt: connection refused"
  ...
  <log stamp> Bumping findnode failure counter         id=8ba26367c9651a53 failcount=2
  ```

## References

* [Go-Ethereum README Operating a Private Network - GitHub](https://github.com/ethereum/go-ethereum#operating-a-private-network)
* [Setting up a local private testnet - Ethereum Homestead](http://ethdocs.org/en/latest/network/test-networks.html#setting-up-a-local-private-testnet)
* [Mining - Go-Ethereum Wiki](https://github.com/ethereum/go-ethereum/wiki/Mining)
* [Contracts and Transactions - Go-Ethereum Wiki](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions)
