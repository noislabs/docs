---
description: >-
  Relayers transfer packets from a chain to another via IBC. They will ship the
  randomness from Nois chain to the consumer chains
---

# 🌉 For IBC relayer operators

Install docker from this [link](https://docs.docker.com/engine/install/ubuntu/)

<pre class="language-shell"><code class="lang-shell"><strong>export MNEMONIC='&#x3C;YOUR_MNEMONICS_HERE>'
</strong>
docker run \
            -e "MNEMONIC=$MNEMONIC" \
            noislabs/nois-relayer:elgafar-1-stars1atajhwmu769z6kp2c4htj3qxxex29rwdh55e686fm4dqc6hz80dsxeld4v \
            ibc-relayer start \
            --src-connection=connection-6 \
            --dest-connection=connection-2 \
            --poll 10</code></pre>
