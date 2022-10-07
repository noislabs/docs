---
description: >-
  Bots bring randomness from drand (https://drand.love/) to the nois blockchain.
  That's the only thing the bot does. After that once that randomness is
  onchain, it will be sold to Dapps accross the cosm
---

# 🤖 For Bot Runners

Install docker from this [link](https://docs.docker.com/engine/install/ubuntu/)

<pre class="language-bash"><code class="lang-bash">#Make sure you have tokens in your wallet
export MNEMONIC='&#x3C;YOUR_MNEMONICS_HERE>'
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois1nc5tatafv6eyq7llkr2gv50ff9e22mnf70qgjlv737ktmt4eswrq5z5suf
export ENDPOINT=https://rpc-t.nois.nodestake.top/
<strong>#Many RPCs are available. For more info check discord #validator channel
</strong><strong>#http://node-0.noislabs.com:26657/
</strong><strong>#https://nois-testnet.rpc.kjnodes.com/
</strong>#https://rpc-t.nois.nodestake.top/
#https://nois.rpc.bccnodes.com/
#http://nois.cryptech.com.ua:26657/

#edit above values before running the docker
docker run \
       -e "MNEMONIC=$MNEMONIC" \
       -e PREFIX=nois \
       -e DENOM=unois \
       -e NOIS_CONTRACT=$NOIS_CONTRACT \
       -e CHAIN_HASH=8990e7a9aaed2ffed73dbd7092123d6f289930540d7651336225dc172e51b2ce \
       -e ENDPOINT=$ENDPOINT \
       -e GAS_PRICE=0.05unois \
       noislabs/nois-bot:latest</code></pre>
