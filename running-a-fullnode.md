---
description: Overview
---

# Running a Fullnode

A coordinated group of validators (maximum: 80) secure the Nois Network. Each validator uses [Tendermint](https://tendermint.com/), a Byzantine fault-tolerant Delegated Proof-of-Stake (DPoS) consensus engine. Each validator stakes their NOIS coins and coins from delegators to earn rewards by successfully running the protocol, verifying transactions, and proposing blocks. If they fail to maintain a consistent (downtime) and honest node (double-signing), slashing penalties will occur, resulting in coins deducted from their account.All NOIS holders can become a Nois Network validator or delegator and participate in staking and governance processes._For information on running a node, delegating, staking, and voting, please see the validator guide below and visit our_ [_governance documentation_](https://docs.scrt.network/protocol/governance.html)_.Here is a list of_ [_SGX-compatible hardware_](https://github.com/ayeks/SGX-hardware) _for running a validator._For detailed information on how to set up and run a validator, see the Validator Set Up section.

## **Slashing For Downtime** <a href="#slashing-for-downtime" id="slashing-for-downtime"></a>

Slashing for downtime is based on actual block times and NOT theoretical block times, such as `SignedBlocksWindow` and `MinSignedPerWindow` network parameters. Validators signing less than MinSignedPerWindow blocks out of every SignedBlocksWindow will experience a downtime slash.

### Slashing Conditions For Downtime <a href="#slashing-conditions-for-downtime" id="slashing-conditions-for-downtime"></a>

Parameters: 11250 blocks out of every 22500-blocks

* For a block time of 6.8 seconds, it roughly translates to being up for less than 21.25 hours out of every 42.5-hour window.
* For a block time of 6.4 seconds, it roughly translates to being up for less than 20 hours out of every 40-hour window.

### Penalties For Downtime <a href="#penalties-for-downtime" id="penalties-for-downtime"></a>

* Slashing of 0.01% of the validator and its delegators' staked NOIS
* Jailing of validators for 10 minutes. Validators do not earn block rewards for the jailing period and must manually unjail their node with:

noisd tx slashing unjail --from \<key-alias>

## **Slashing For Double-Signing** <a href="#slashing-for-double-signing" id="slashing-for-double-signing"></a>

Slashing for double-signing is when the validator signs the same block twice. This happens most commonly during migration, and the original validator node is not shut off appropriately, and their `priv_validator_key.json` is running on two machines at the same time. The best way to avoid double signing is by using a remote signer such as the [TenderMint Key Management System](https://github.com/iqlusioninc/tmkms) (TMKMS) or [Horcrux](https://github.com/strangelove-ventures/horcrux).

### Conditions **F**or Double-Signing <a href="#conditions-for-double-signing" id="conditions-for-double-signing"></a>

* A validator signs the same block height twice

### Penalties For Double-Signing <a href="#penalties-for-double-signing" id="penalties-for-double-signing"></a>

* Slashing of 5% of the validator and its delegators' staked NOIS
* Jailing forever (tombstoned) of the validator node
  * A tombstoned validator cannot earn block rewards anymore, and its delegators must re-delegate their NOIS to another validator

## What Happens With Slashed NOIS <a href="#what-happens-with-slashed-scrt" id="what-happens-with-slashed-scrt"></a>
