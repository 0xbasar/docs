---
title: 'Network parameters'
sidebar_position: 1
description: 'Network Parameters for LUKSO Mainnet: Execution Block Explorer, RPC providers, Chain ID.'
---

import AddNetworkButton from '../../../src/components/AddNetworkButton'

# Network Parameters

| Setting                  | Value                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------ |
| Network Name             | Mainnet                                                                                                      |
| Genesis Fork Version     | 0x42000001                                                                                                   |
| Chain ID / Network ID    | 42                                                                                                           |
| Currency Symbol          | LYX                                                                                                          |
| Execution Block Explorer | [https://explorer.execution.mainnet.lukso.network](https://explorer.execution.mainnet.lukso.network)         |
| Blockscout API           | [https://explorer.execution.mainnet.lukso.network/api](https://explorer.execution.mainnet.lukso.network/api) |
| Execution Status Page    | [https://stats.execution.mainnet.lukso.network](https://stats.execution.mainnet.lukso.network)               |
| Consensus Block Explorer | [https://explorer.consensus.mainnet.lukso.network](https://explorer.consensus.mainnet.lukso.network)         |
| Consensus Status Page    | [https://stats.consensus.mainnet.lukso.network](https://stats.consensus.mainnet.lukso.network)               |
| Launchpad                | [https://deposit.mainnet.lukso.network](https://deposit.mainnet.lukso.network)                               |
| Checkpoints              | [https://checkpoints.mainnet.lukso.network](https://checkpoints.mainnet.lukso.network)                       |

The mainnet network configs are defined or the [`lukso-network/network-configs`](https://github.com/lukso-network/network-configs/tree/main/mainnet/shared) repo.

## 3rd party RPC providers

Developers can use the services of the following 3rd party providers:

- [Gateway.fm](https://gateway.fm/) RPC URL: `https://rpc.lukso.gateway.fm`
- [NowNodes](https://nownodes.io/) RPC URL: `https://lukso.nownodes.io`
- [Thirdweb](https://thirdweb.com/) RPC URL: `https://lukso.rpc.thirdweb.com`

## Add LUKSO network (MetaMask...)

<AddNetworkButton/>

You can add LUKSO as a custom network in your existing wallet (MetaMask, Rainbow, Coinbase Wallet, Trust Wallet, etc.) with the parameters below:

| Setting               | Value                                             |
| --------------------- | ------------------------------------------------- |
| Network Name          | LUKSO                                             |
| RPC URL               | https://rpc.lukso.gateway.fm                      |
| Chain ID / Network ID | 42                                                |
| Currency Symbol       | LYX                                               |
| Block explorer URL    | https://explorer.execution.mainnet.lukso.network/ |

:::tip Hardware wallets

You can use your hardware wallet with MetaMask. You will simply need to:

1. Connect your hardware wallet to MetaMask
2. Add LUKSO as a custom network (cf. settings above)
3. For Ledger users: Download the Ethereum app on your Ledger
4. Switch the network to "LUKSO"

:::

## Network Architecture

LUKSO's Blockchain Architecture runs the Ethereum protocol and consists of 2 to 4 clients:

- The consensus client running Casper the Friendly Finality Gadget (Casper FFG) plus LMD-GHOST fork choice algorithm ([More on the Gasper Consensus](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/gasper/))
- The execution client, [running the Ethereum Virtual Machine](https://ethereum.org/en/developers/docs/ethereum-stack/)
