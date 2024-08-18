# The Aptos Handbook

Welcome. This is EasyA's legendary handbook. If you want to learn Aptos like a legend, then you're in the right place.

# Purpose

This handbook serves as a guide to the Aptos ecosystem, geared towards those just joining for the first time. It isn't just a beginners' handbook; it's a legendary handbook. Even if you've already immersed yourself in the ecosystem, you'll find tons of helpful tidbits around here!

# The EasyA App

Of course, the best place to start is always the [EasyA](https://www.easya.io) app! Download it here for the fastest and most fun way to learn about Aptos. Download it directly right [here](https://links.easya.io/links/gotoapp)!

## Table of Contents

- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Core Concepts](#core-concepts)
- [Development Tools](#development-tools)
- [Smart Contracts](#smart-contracts)
- [Aptos Network](#aptos-network)
- [Ecosystem Projects](#ecosystem-projects)
- [Resources](#resources)
- [Handy Code Snippets](#handy-code-snippets)
- [Contributing](#contributing)

## Introduction

What is Aptos:

- [Aptos Website](https://aptoslabs.com/) - The official website for Aptos.

## Getting Started

The no-fluff starter:

- [Installation](https://aptos.dev/tools/install-cli/) - Official guide to install the Aptos CLI and start developing.
- [Aptos White Paper](https://aptos.dev/aptos-white-paper/) - Comprehensive overview of Aptos's architecture and vision.
- [Your First Transaction](https://aptos.dev/tutorials/your-first-transaction) - Tutorial to get you started with your first transaction on Aptos.

## Core Concepts

Explanation of fundamental concepts in the Aptos ecosystem:

- [Move on Aptos](https://aptos.dev/move/move-on-aptos) - An introduction to the Move programming language as used in Aptos.
- [Move Book](https://move-book.com/) - Comprehensive guide to the Move programming language, which Aptos is built upon.

## Development Tools

Key tools and environments for Aptos:

- [Move VS Code plugin](https://marketplace.visualstudio.com/items?itemName=move.move-analyzer) - Enhances the development experience for Move in Visual Studio Code.
- [Aptos CLI](https://aptos.dev/tools/install-cli/) - Command-line interface for interacting with the Aptos network.
- [Aptos Explorer](https://explorer.aptoslabs.com/) - Official block explorer for the Aptos network.

## Smart Contracts

How to write and deploy smart contracts on Aptos:

- [Your First Move Module](https://aptos.dev/tutorials/first-move-module) - Guide to writing and deploying your first Move module on Aptos.
- [Implementing a Fungible Token](https://github.com/move-language/move/tree/main/language/documentation/tutorial) - Tutorial on implementing, testing, and verifying a fungible token.

## Aptos Network

Going into the network level:

- [Aptos Mainnet](https://aptoslabs.com/community) - Information on Aptos's mainnet and how to participate.
- [Aptos Devnet](https://aptos.dev/nodes/aptos-deployments/) - Details on Aptos's devnet for development and testing.

## Ecosystem Projects

Cool projects built on Aptos:

- [Petra Aptos Wallet](https://petra.app/) - Official wallet for managing assets on the Aptos network.
- [Topaz](https://www.topaz.so/) - NFT Marketplace built on Aptos.
- [PancakeSwap](https://aptos.pancakeswap.finance/swap) - Decentralized exchange on Aptos.

...and of course many more - check them out in the EasyA app!

## Resources

Extra stuff:

- [Aptos Developer Documentation](https://aptos.dev/) - Official documentation for Aptos developers.
- [Aptos GitHub](https://github.com/aptos-labs/aptos-core) - The main repository for Aptos's development.

## Handy Code Snippets

Get a taste of Aptos development with these code snippets:

### Initializing a coin

```move
module 0x1::coin {
    public fun initialize<CoinType>(
        account: &signer,
        name: string::String,
        symbol: string::String,
        decimals: u8,
        monitor_supply: bool,
    ): (BurnCapability<CoinType>, FreezeCapability<CoinType>, MintCapability<CoinType>) {
        let account_addr = signer::address_of(account);
 
        assert!(
            coin_address<CoinType>() == account_addr,
            error::invalid_argument(ECOIN_INFO_ADDRESS_MISMATCH),
        );
 
        assert!(
            !exists<CoinInfo<CoinType>>(account_addr),
            error::already_exists(ECOIN_INFO_ALREADY_PUBLISHED),
        );
 
        let coin_info = CoinInfo<CoinType> {
            name,
            symbol,
            decimals,
            supply: if (monitor_supply) { option::some(optional_aggregator::new(MAX_U128, false)) } else { option::none() },
        };
        move_to(account, coin_info);
 
        (BurnCapability<CoinType>{ }, FreezeCapability<CoinType>{ }, MintCapability<CoinType>{ })
  }
}
```

### Minting a coin

```move
module 0x1::coin {
    public fun mint<CoinType>(
        amount: u64,
        _cap: &MintCapability<CoinType>,
    ): Coin<CoinType> acquires CoinInfo {
        if (amount == 0) {
            return zero<CoinType>()
        };
 
        let maybe_supply = &mut borrow_global_mut<CoinInfo<CoinType>>(coin_address<CoinType>()).supply;
        if (option::is_some(maybe_supply)) {
            let supply = option::borrow_mut(maybe_supply);
            optional_aggregator::add(supply, (amount as u128));
        };
 
        Coin<CoinType> { value: amount }
    }
}
```



These examples showcase:
1. How to initialize a coin on Aptos.
2. How to mint a coin on Aptos.

## Contributing

We welcome contributions to make this handbook even more legendary! Here's how you can contribute:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/amazing-addition`)
3. Make your changes
4. Commit your changes (`git commit -am 'Add some amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-addition`)
6. Create a new Pull Request

Please ensure your contributions align with our code of conduct and contribution guidelines.

## Credit/Inspiration

This handbook was inspired by the famous awesome lists by sindresorhus and the Awesome Aptos list. We need awesome lists for Web3 ecosystems, with more of a hacker's guide to how they work. This is the answer to that need.
