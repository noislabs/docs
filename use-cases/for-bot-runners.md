---
description: >-
  Bots bring randomness from drand (https://drand.love/) to the Nois blockchain.
  That's the only thing the bot does. Once that randomness is on chain, it will
  be offered to Dapps accross the Cosmos.
---

# 🤖 For Bot Runners



1. Install docker as explained [here](https://docs.docker.com/engine/install/ubuntu/)
2. Download the latest version of the bot image: `docker pull noislabs/nois-bot:latest`
3. Run the bot as follows:

<pre class="language-bash"><code class="lang-bash">#Make sure you have tokens in your wallet
export MNEMONIC='&#x3C;YOUR_MNEMONICS_HERE>'
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois1j7m4f68lruceg5xq3gfkfdgdgz02vhvlq2p67vf9v3hwdydaat3sajzcy5
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
