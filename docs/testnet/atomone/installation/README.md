---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: atomone-testnet-1 | **Latest Version Tag**: v1.0.1  | **Custom Port**: 10

```
echo "export NODENAME=Yournodename" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=atomone-testnet-1" >> $HOME/.bash_profile
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
VER="1.21.3"
wget "https://golang.org/dl/go1.21.3.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go1.21.3.linux-amd64.tar.gz"
rm "go1.21.3.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

## Download and build binaries
```
cd $HOME
wget https://server-1.ruangnode.com/testnet/atomone/atomoned
chmod +x atomoned
mv atomoned $HOME/go/bin/atomone
```

## Init app
```
atomoned init Yournodename --chain-id atomone-testnet-1
```

## Download configuration
```
cd $HOME
wget -O $HOME/.atomone/config/genesis.json https://server-1.ruangnode.com/testnet/atomone/genesis.json
wget -O $HOME/.atomone/config/addrbook.json https://server-1.ruangnode.com/testnet/atomone/addrbook.json
```

```
SEEDS="85e441cfe74b8c0f8b820beff46edab20e92716c@atomone-testnet-seed.itrocket.net:62657"
PEERS="9c2e0452539d913214048111afd4872ea2edd32f@65.108.206.118:61356"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.atomone/config/config.toml
```

```
# set custom ports in app.toml
sed -i.bak -e "s%:1317%:$\{CUSTOM_PORT}317%g;
s%:8080%:${CUSTOM_PORT}080%g;
s%:9090%:${CUSTOM_PORT}090%g;
s%:9091%:${CUSTOM_PORT}091%g;
s%:8545%:${CUSTOM_PORT}545%g;
s%:8546%:${CUSTOM_PORT}546%g;
s%:6065%:${CUSTOM_PORT}065%g" $HOME/.atomone/config/app.toml
```

```
# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${CUSTOM_PORT}658%g;
s%:26657%:${CUSTOM_PORT}657%g;
s%:6060%:${CUSTOM_PORT}060%g;
s%:26656%:${CUSTOM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CUSTOM_PORT}656\"%;
s%:26660%:${CUSTOM_PORT}660%g" $HOME/.atomone/config/config.toml
```

## Disable indexing
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.atomone/config/config.toml
```

## Config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.atomone/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.atomone/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.atomone/config/app.toml
```

## Set minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025uatone"|g' $HOME/.atomone/config/app.toml
```

## Create service
```
sudo tee /etc/systemd/system/atomoned.service > /dev/null <<EOF2
[Unit]
Description=atomoned Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which atomoned) start
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
sudo systemctl enable atomoned
sudo systemctl restart atomoned && sudo journalctl -u atomoned -f -o cat
```
