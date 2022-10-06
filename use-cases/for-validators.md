# 🔐 For validators

Update your OS

```shell
apt-get update 
sudo apt install -y build-essential
```

Install go [https://go.dev/doc/install](https://go.dev/doc/install)

Clone the nois full node repository

```shell
git clone https://github.com/noislabs/full-node.git 
cd full-node/full-node/
```

&#x20;Install the noisd binary

```shell
./setup.sh
#This will create the binary under out/noisd
#move the binary under your go bin directory
mkdir -p $HOME/go/bin
mv out/noisd $HOME/go/bin/
export PATH=$HOME/go/bin:$PATH
```

Adapt the block time and minimum gas prices parameters

```shell
export DENOM=unois
export CONFIG_DIR=$HOME/.noisd/config
sed -i 's/minimum-gas-prices = ""/minimum-gas-prices = "0.05'"${DENOM}"'"/' $CONFIG_DIR/app.toml
    sed -i 's/^timeout_propose =.*$/timeout_propose = "1s"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_propose_delta =.*$/timeout_propose_delta = "200ms"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_prevote =.*$/timeout_prevote = "500ms"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_prevote_delta =.*$/timeout_prevote_delta = "200ms"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_precommit =.*$/timeout_precommit = "1s"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_precommit_delta =.*$/timeout_precommit_delta = "200ms"/' $CONFIG_DIR/config.toml
    sed -i 's/^timeout_commit =.*$/timeout_commit = "3s"/' $CONFIG_DIR/config.toml
```

&#x20; Check the installation

```shell
noisd version
```

The rest is similar to running a standard cosmos validator. You can check this cosmos hub docs [link](https://hub.cosmos.network/main/validators/validator-setup.html) for more details

For the faucet, rpc links, permanent peers and similar details visit [networks-and-contracts](../networks-and-contracts/ "mention")



