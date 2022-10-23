---
description: >-
  Relayers transfer packets from a chain to another via IBC. They will ship the
  randomness from Nois chain to the consumer chains
---

# 🌉 For IBC relayer operators

Install docker from this [link](https://docs.docker.com/engine/install/ubuntu/)

For elgafar-1 => nois relay

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

For uni-5 => nois relay

```shell
export MNEMONIC='<YOUR_MNEMONICS_HERE>'

docker run \
            -e "MNEMONIC=$MNEMONIC" \
            docker.io/noislabs/nois-relayer:uni-5-juno1tquqqdvlv3fwu5u6evpt7e4ss47zczug8tq4czjucgx8dulkhjxsegfuds \
            ibc-relayer start \
            --src-connection=connection-39 \
            --dest-connection=connection-12 \
            --poll 3
```
