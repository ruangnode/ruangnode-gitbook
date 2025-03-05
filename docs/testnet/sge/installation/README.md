---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/sge.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: sge-network-4 | **Latest Version Tag**: v1.7.6  | **Custom Port**: 10

```
echo "export NODENAME=Yournodename" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=sge-network-4" >> $HOME/.bash_profile
echo "export CUSTOM_PORT="10"" >> /home/github/.bash_profile
source $HOME/.bash_profile
```

## Update packages
```
sudo apt update && sudo apt upgrade -y
```

## Install dependencies
```
sudo apt install curl build-essential git wget jq make gcc tmux net-tools ccze -y
```

## Install Go
```
cd $HOME
VER="1.23.1"
wget "https://golang.org/dl/go1.23.1.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go1.23.1.linux-amd64.tar.gz"
rm "go1.23.1.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

## Download and build binaries
```
cd $HOME
wget https://server-1.ruangnode.com/testnet/sge/sged
chmod +x sged
mv sged $HOME/go/bin/sge
```

## Init app
```
sged init Yournodename --chain-id sge-network-4
```

## Download configuration
```
cd $HOME
wget -O $HOME/.sge/config/genesis.json https://server-1.ruangnode.com/testnet/sge/genesis.json
wget -O $HOME/.sge/config/addrbook.json https://server-1.ruangnode.com/testnet/sge/addrbook.json
```

```
SEEDS=""
PEERS="7128e7b73c131c053e9dfbd1e68d977abfe61dfb@213.239.207.175:13256,68d1cf8ffe9d8a27250c26da69f038d575b4f1a9@78.46.103.246:60956,6230c5aba898897663f9fb4d65796a187ce2f571@167.235.12.38:12656,5168a051191bbd05d639b9091653c92f43f0cd11@89.58.13.159:35656,c30b391ae80e9ef4ba0659404b21e3f20530efa3@65.108.134.47:17756,e6fd6f254a79391298a1577499a03c7989e1a578@65.108.230.113:1146,145d0f311ef1485f5b95eebecbc758fce01b4bb6@38.146.3.184:17756,51e4e7b04d2f669f5efa53e8d95891fa04e4c5b9@206.125.33.62:26656,64f3549294c57f23b39cd9bfd0a9f8abb6186ca1@65.21.32.216:36656,b007233c39e437b1d2ca1d77464ef55a9194193e@52.44.14.245:26656,6caabc35628a51bbf9c80ead303f13b3dfae8674@50.19.180.153:26656,a4f4f2bae118c32d679acb3490e68c07f95da6f4@65.109.112.148:1156"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.sge/config/config.toml
```

```
# set custom ports in app.toml
sed -i.bak -e "s%:1317%:$\{CUSTOM_PORT}317%g;
s%:8080%:${CUSTOM_PORT}080%g;
s%:9090%:${CUSTOM_PORT}090%g;
s%:9091%:${CUSTOM_PORT}091%g;
s%:8545%:${CUSTOM_PORT}545%g;
s%:8546%:${CUSTOM_PORT}546%g;
s%:6065%:${CUSTOM_PORT}065%g" $HOME/.sge/config/app.toml
```

```
# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${CUSTOM_PORT}658%g;
s%:26657%:${CUSTOM_PORT}657%g;
s%:6060%:${CUSTOM_PORT}060%g;
s%:26656%:${CUSTOM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CUSTOM_PORT}656\"%;
s%:26660%:${CUSTOM_PORT}660%g" $HOME/.sge/config/config.toml
```

## Disable indexing
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.sge/config/config.toml
```

## Config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.sge/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.sge/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.sge/config/app.toml
```

## Set minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025usge"|g' $HOME/.sge/config/app.toml
```

## Create service
```
sudo tee /etc/systemd/system/sged.service > /dev/null <<EOF2
[Unit]
Description=sged Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which sged) start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF2
```

## Register and start service
```
sudo systemctl daemon-reload
sudo systemctl enable sged
sudo systemctl restart sged && sudo journalctl -u sged -f -o cat
```
