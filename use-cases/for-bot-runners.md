---
description: >-
  Bots bring randomness from drand to the Nois blockchain. That's the only thing
  the bot does. Once that randomness is on chain, it will be offered to Dapps
  across the Cosmos.
---

# 🤖 For Bot Runners

### Using a Docker image

1. Install Docker as explained [here](https://docs.docker.com/engine/install/ubuntu/)
2. Download the latest version of the bot image: `docker pull noislabs/nois-bot:latest`
3. Run the bot as follows:

<pre class="language-bash"><code class="lang-bash">#Make sure you have tokens in your wallet
export MNEMONIC='&#x3C;YOUR_MNEMONICS_HERE>'
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois1j7m4f68lruceg5xq3gfkfdgdgz02vhvlq2p67vf9v3hwdydaat3sajzcy5
export ENDPOINT=https://rpc.noislabs.com:443
export MONIKER=your-beautiful-name
<strong>#Many RPCs are available. For more info check discord #validator channel
</strong>#http://node-0.noislabs.com:26657/
<strong>#https://nois-testnet.rpc.kjnodes.com/
</strong>#https://rpc-t.nois.nodestake.top/
#https://nois.rpc.bccnodes.com/
#http://nois.cryptech.com.ua:26657/

#edit above values before running the docker
docker run \
       -e MONIKER=$MONIKER \
       -e "MNEMONIC=$MNEMONIC" \
       -e PREFIX=nois \
       -e DENOM=unois \
       -e NOIS_CONTRACT=$NOIS_CONTRACT \
       -e CHAIN_HASH=8990e7a9aaed2ffed73dbd7092123d6f289930540d7651336225dc172e51b2ce \
       -e ENDPOINT=$ENDPOINT \
       -e GAS_PRICE=0.05unois \
       noislabs/nois-bot:latest</code></pre>

### Using a plain Node.js script

The bot can also be run without containerization. This requires some Node.js knowledge and is more manual work. But it gives operators more control and is especially useful for debugging. Instructions are maintained here: [https://github.com/noislabs/nois-bot/blob/main/RUN\_ON\_SERVER.md](https://github.com/noislabs/nois-bot/blob/main/RUN\_ON\_SERVER.md).
