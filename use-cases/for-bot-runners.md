---
description: >-
  Bots bring randomness from drand to the Nois blockchain. That's the only thing
  the bot does. Once that randomness is on chain, it will be offered to Dapps
  across the Cosmos.
---

# 🤖 For Bot Runners

{% hint style="info" %}
Drand-bots do not and cannot generate randomness. The simply relay it from drand to nois chains.
{% endhint %}

{% hint style="info" %}
Drand-bots are never slashed and do not need to put any collateral simply because they cannot cheat.
{% endhint %}

{% hint style="warning" %}
Although drand-bots cannot impact the outcome of the randomness, they can impact the availability of the randomness by stopping to work or by failing to submit to the nois chain. \
This is why they are incentivised and constitute a critical piece of nois.
{% endhint %}

### Using a Docker image

1. Install Docker as explained [here](https://docs.docker.com/engine/install/ubuntu/)
2. Download the latest version of the bot image: `docker pull noislabs/nois-bot:latest`
3. Run the bot as follows:

<pre class="language-bash"><code class="lang-bash">#Make sure you have tokens in your wallet
export MNEMONIC='&#x3C;YOUR_MNEMONICS_HERE>'
#check https://docs.nois.network/networks-and-contracts. nois-oracle contract
export NOIS_CONTRACT=nois1s9ly26evj8ehurptws5d6dm4a9g2z0htcqvlvn95kc30eucl4s5sd8hkgp
export ENDPOINT=https://nois.rpc.bccnodes.com:443
export MONIKER=your-beautiful-name
<strong>#Many RPCs are available. For more info check discord #validator channel
</strong><strong>#https://nois-testnet.rpc.kjnodes.com/
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

### Performance factors of drand-bots

the main criteria in running a fast drand-bot is being able to broadcast your tx as fast as possible in order to reach the proposer's node asap before the other drand-bots. \
this will allow your submission tx to be placed among the first 6 bots in the mempool of the proposer's node.\
Obviously you need a good cpu and a fast connection that is very close to an rpc node.\
Your rpc node needs to be fast in the network and eventually needs to have fast direct peers so it can reach the proposer before the competitor drand-bot operators.

Choosing an rpc node that is heavily used or that many other bot operators are using would not be optimal. Using an rpc that has big voting power can slightly increase your chances because that node gets chosen to propose blocks a bit more often than other validator nodes so sometimes you get that small advantage when your RPC is the block proposer. But this is a very small difference and often validator nodes do not offer an rpc endpoint.

In general if you get like +3 second delay it is not because your bot submitted the tx 3 seconds later than the other drand-bots but because they were few ms before you so the proposer did not choose to select you in the proposed block because there is a consensus blockspace that only allows 4-5 submissions. so your submission becomes leftover for the next block (in 3 seconds) and if you also miss the second block because there are more than 10 operators faster than your drand-bot. then your submission only gets included in the block afterwards so +6seconds.
