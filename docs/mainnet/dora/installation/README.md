---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/dora.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: vota-ash | **Latest Version Tag**: 0.4.3 | **Custom Port**: 10

```
echo 'export NODENAME=Yournodename' >> $HOME/.bash_profile
echo 'export WALLET=wallet' >> $HOME/.bash_profile
echo 'export CHAIN_ID=vota-ash' >> $HOME/.bash_profile
echo 'export CUSTOM_PORT=10' >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Update packages
```
sudo apt update && sudo apt upgrade -y
```

## Install dependencies
```
sudo apt install -y curl build-essential git wget jq make gcc tmux net-tools ccze lz4
```

## Install Go
```
cd $HOME
VER="1.21.13"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:~/go/bin' >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

## Download and install binary
```
cd $HOME
wget -O dorad https://server-1.ruangnode.com/mainnet/doravota/dorad
chmod +x dorad
mv dorad $HOME/go/bin/dorad
```

## Init app
```
dorad init "$NODENAME" --chain-id vota-ash --home $HOME/.dora
```

## Download configuration
```
mkdir -p $HOME/.dora/config
wget -O $HOME/.dora/config/genesis.json https://server-1.ruangnode.com/mainnet/doravota/genesis.json
wget -O $HOME/.dora/config/addrbook.json https://server-1.ruangnode.com/mainnet/doravota/addrbook.json
```

### Set peers & seeds
```
SEEDS="19fb0ecb90dc40b741ea1d702c681d3c1a75d643@54.169.53.97:26656"
PEERS="19fb0ecb90dc40b741ea1d702c681d3c1a75d643@54.169.53.97:26656"
CONFIG=$HOME/.dora/config/config.toml
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"19fb0ecb90dc40b741ea1d702c681d3c1a75d643@54.169.53.97:26656\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"19fb0ecb90dc40b741ea1d702c681d3c1a75d643@54.169.53.97:26656\"/}" $CONFIG
```

### Custom ports
```
APP=$HOME/.dora/config/app.toml
CFG=$HOME/.dora/config/config.toml

# app.toml
sed -i.bak -e "s%:1317%:10317%g; s%:8080%:10080%g; s%:9090%:10090%g; s%:9091%:10091%g; s%:8545%:10545%g; s%:8546%:10546%g; s%:6065%:10065%g" $APP

# config.toml
sed -i.bak -e "s%:26658%:10658%g; s%:26657%:10657%g; s%:6060%:10060%g; s%:26656%:10656%g; s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):10656\"%; s%:26660%:10660%g" $CFG
```

### Disable indexing (optional)
```
sed -i 's/^indexer *=.*/indexer = "null"/' $HOME/.dora/config/config.toml
```

### Pruning
```
sed -i 's/^pruning *=.*/pruning = "custom"/' $HOME/.dora/config/app.toml
sed -i 's/^pruning-keep-recent *=.*/pruning-keep-recent = "100"/' $HOME/.dora/config/app.toml
sed -i 's/^pruning-interval *=.*/pruning-interval = "19"/' $HOME/.dora/config/app.toml
```

### Minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "10000000000peaka"|g' $HOME/.dora/config/app.toml
```

## Create service
```
sudo tee /etc/systemd/system/dorad.service >/dev/null <<EOF2
[Unit]
Description=dorad Node
After=network-online.target

[Service]
User=${USER}
ExecStart=$(which dorad) start --home $HOME/.dora
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF2
```

## Enable & start
```
sudo systemctl daemon-reload
sudo systemctl enable dorad
sudo systemctl restart dorad && sudo journalctl -u dorad -f -o cat
```
