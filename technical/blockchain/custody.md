# A Full CryptoCurrency Custody Solution Based on MPC and Threshold ECDSA

compiled from https://rwc.iacr.org/2019/slides/Multiparty-ECDSA-RWC2019.pdf

## Motivation

![](https://images.squarespace-cdn.com/content/v1/586cf12903596e5605548ae1/1558399977091-P73ZQ5ATOEAOIUIK984G/ke17ZwdGBToddI8pDm48kDSItL1-z1GOo9m7MQ7318AUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcIjBxpvhyQL7XDHoxeflpQp_bNTjsO8XwxU16-jXqhmo-nFSAP4jV8ARxqQoMXVzM/Signatures+Comparative+Matrix.png?format=1500w)

## Custody and Protection

+ Cryptocurrency protection needs
    * Exchanges
        - High turnover – need to speed up transactions
            + Can take days to weeks today on exchanges
        - Separation of vaults (large, medium, small)
            + Higher protection on larger vaults
    * Custody solutions (for banks/financial institutions)
        - Small turnover – complex transactions acceptable (and desired)
        - Very large amount of funds
        - Offered to high-end customers
    * Wallets
        - For end users, small amounts of funds

## Solution Platform Requirements

+ High security
    * Protection against key theft and fraudulent key usage
+ Backup and disaster recovery
+ Flexibility
    * Fine tune security vs usability (ease of transfer)
    * Broad support
        - Different coins/systems
        - Different signing algorithms
        - Standards (e.g., BIP032/BIP044)

## Cryptographic Core – Threshold Signing

+ Multiparty protocols with full threshold security for malicious adversaries
    * Support ECDSA, EdDSA, Schnorr
    * Supports distributed key generation
    * Achieves proactive security (post-compromise security)
+ Rich access structures supported for all
    * Support AND/OR of sets of parties
    * Different structures for different levels of sensitivity/security
+ Two types of parties
    * Online signing parties: run actual MPC protocol (hold subset of shares)
    * Offline authorization parties: approve operation and provide their shares to online parties to carry out protocol

## Custody Setting

__TODO__

## Threshold ECDSA

## 协议对比

+ 两方
+ 多方


## A Protocol vs a Platform

__TODO__

### Additional Components

__TODO__

## Summary

__TODO__



