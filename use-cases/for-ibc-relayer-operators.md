---
description: >-
  Relayers transfer packets from a chain to another via IBC. They will ship the
  randomness from Nois chain to the consumer chains
---

# 🌉 For IBC relayer operators

{% hint style="info" %}
In this tutorial we are using the ts-relayer by confio as a relayer software.\
However, feel free to use other relayer software like Hermes and if you want to contribute to the docs by providing the steps for setting up such different software let us know on discord
{% endhint %}

{% hint style="warning" %}
We are not doing token transfer ICS 20 yet. The relayers we are currently operating on the wasm ports and they are relaying randomness data not tokens
{% endhint %}

### Setting up ts-relayer

\
Install docker from this [link](https://docs.docker.com/engine/install/ubuntu/)

#### &#x20;elgafar-1 => nois relay

```shell
export MNEMONIC='<YOUR_MNEMONICS_HERE>'

docker run \
            -e "MNEMONIC=$MNEMONIC" \
            noislabs/nois-relayer:elgafar-1-stars1caqz4ye9uwm5mtqnyg6vtvqq2gkawlc95vn2c3ede3xxhsrk55fsggxw3u \
            ibc-relayer start \
            --src-connection=connection-18 \
            --dest-connection=connection-10 \
            --poll 3
```

#### uni-5 => nois-testnet-003 relay

```shell
export MNEMONIC='<YOUR_MNEMONICS_HERE>'

docker run \
            -e "MNEMONIC=$MNEMONIC" \
            docker.io/noislabs/nois-relayer:uni-5-juno1v82su97skv6ucfqvuvswe0t5fph7pfsrtraxf0x33d8ylj5qnrysdvkc95 \
            ibc-relayer start \
            --src-connection=connection-267 \
            --dest-connection=connection-28 \
            --poll 3
```

#### &#x20;juno-1 => nois-testnet-003 relay

[https://ibc.nois.network/connections/connection-28/channels/wasm.nois1s9ly26evj8ehurptws5d6dm4a9g2z0htcqvlvn95kc30eucl4s5sd8hkgp:channel-20](https://ibc.nois.network/connections/connection-28/channels/wasm.nois1s9ly26evj8ehurptws5d6dm4a9g2z0htcqvlvn95kc30eucl4s5sd8hkgp:channel-20)

```shell
export MNEMONIC='<YOUR_MNEMONICS_HERE>'

docker run \
            -e "MNEMONIC=$MNEMONIC" \
            noislabs/nois-relayer:juno-1-juno1380xsz7qnanpn9h3zdy97yp5ga98hw32kkswl2wu6qgv8m4g9lms3kjn8y \
            ibc-relayer start \
            --src-connection=<CONNECTION> \
            --dest-connection=<CONNECTION>\
            --poll 3
```
