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

&#x20;  Check the installation

```shell
noisd version
```

The rest is similar to running a standard cosmos validator. You can check this cosmos hub docs [link](https://hub.cosmos.network/main/validators/validator-setup.html) for more details

For the faucet, rpc links, permanent peers and similar details visit [networks-and-contracts](../networks-and-contracts/ "mention")



