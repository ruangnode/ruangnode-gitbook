---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/selfchain.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: sge-selfchain-testnet | **Latest Version Tag**: v1.0.1  | **Custom Port**: 13

```
echo "export NODENAME=Yournodename" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=sge-selfchain-testnet" >> $HOME/.bash_profile
echo "export CUSTOM_PORT="13"" >> /home/github/.bash_profile
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
wget https://server-1.ruangnode.com/testnet/selfchain/selfchaind-v1.0.1
mv selfchaind-v1.0.1 selfchaind
chmod +x selfchaind
mv selfchaind $HOME/go/bin/selfchain
```

## Init app
```
selfchaind init Yournodename --chain-id sge-selfchain-testnet
```

## Download configuration
```
cd $HOME
wget -O $HOME/.selfchain/config/genesis.json https://server-1.ruangnode.com/testnet/selfchain/genesis.json
wget -O $HOME/.selfchain/config/addrbook.json https://server-1.ruangnode.com/testnet/selfchain/addrbook.json
```

```
SEEDS=""
PEERS="54bc6768af48108d7c50344362165c7870d89275@37.27.52.25:13656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.selfchain/config/config.toml
```

```
# set custom ports in app.toml
sed -i.bak -e "s%:1317%:$\{CUSTOM_PORT}317%g;
s%:8080%:${CUSTOM_PORT}080%g;
s%:9090%:${CUSTOM_PORT}090%g;
s%:9091%:${CUSTOM_PORT}091%g;
s%:8545%:${CUSTOM_PORT}545%g;
s%:8546%:${CUSTOM_PORT}546%g;
s%:6065%:${CUSTOM_PORT}065%g" $HOME/.selfchain/config/app.toml
```

```
# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${CUSTOM_PORT}658%g;
s%:26657%:${CUSTOM_PORT}657%g;
s%:6060%:${CUSTOM_PORT}060%g;
s%:26656%:${CUSTOM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CUSTOM_PORT}656\"%;
s%:26660%:${CUSTOM_PORT}660%g" $HOME/.selfchain/config/config.toml
```

## Disable indexing
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.selfchain/config/config.toml
```

## Config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.selfchain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.selfchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.selfchain/config/app.toml
```

## Set minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "200000uself"|g' $HOME/.selfchain/config/app.toml
```

## Create service
```
sudo tee /etc/systemd/system/selfchaind.service > /dev/null <<EOF2
[Unit]
Description=selfchaind Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which selfchaind) start
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
sudo systemctl enable selfchaind
sudo systemctl restart selfchaind && sudo journalctl -u selfchaind -f -o cat
```
