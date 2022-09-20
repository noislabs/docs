# 🤖 For Bot Runners

Install docker from this [link](https://docs.docker.com/engine/install/ubuntu/)

```bash
export MNEMONIC=<YOUR_MNEMONICS_HERE>
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrq5z5suf
export ENDPOINT=http://rpc-0.noislabs.com:26657
#edit above values before running the docker
docker run \
       -e MNEMONIC=$MNEMONIC \
       -e PREFIX=nois \
       -e DENOM=unois \
       -e NOIS_CONTRACT=$NOIS_CONTRACT \
       -e CHAIN_HASH=8990e7a9aaed2ffed73dbd7092123d6f289930540d7651336225dc172e51b2ce \
       -e ENDPOINT=$ENDPOINT \
       -e GAS_PRICE=0.05unois \
       noislabs/nois-bot:latest
```
