---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/fuelsequencer.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: seq-mainnet-1 | **Latest Version Tag**: seq-mainnet-1.2-improved-sidecar  | **Custom Port**: 10

```
echo "export NODENAME=Yournodename" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=seq-mainnet-1" >> $HOME/.bash_profile
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
  wget "https://golang.org/dl/go1.21.3.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go1.21.3.linux-amd64.tar.gz"
  rm "go1.21.3.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

## Download and build binaries
```
cd $HOME
wget https://server-1.ruangnode.com/mainnet/fuelsequencer/fuelsequencerd
chmod +x fuelsequencerd
mv fuelsequencerd $HOME/go/bin/fuelsequencer
```

## Init app
```
fuelsequencerd init Yournodename --chain-id seq-mainnet-1
```

## Download configuration
```
cd $HOME
wget -O $HOME/.fuelsequencer/config/genesis.json https://server-1.ruangnode.com/mainnet/fuelsequencer/genesis.json
wget -O $HOME/.fuelsequencer/config/addrbook.json https://server-1.ruangnode.com/mainnet/fuelsequencer/genesis.json
```

```
SEEDS=""
PEERS=""
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
s%:6065%:${CUSTOM_PORT}065%g" $HOME/.fuelsequencer/config/app.toml
```

```
# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${CUSTOM_PORT}658%g;
s%:26657%:${CUSTOM_PORT}657%g;
s%:6060%:${CUSTOM_PORT}060%g;
s%:26656%:${CUSTOM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CUSTOM_PORT}656\"%;
s%:26660%:${CUSTOM_PORT}660%g" $HOME/.fuelsequencer/config/config.toml
```

## Disable indexing
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.fuelsequencer/config/config.toml
```

## Config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.fuelsequencer/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.fuelsequencer/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.fuelsequencer/config/app.toml
```

## Set minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "10fuel"|g' $HOME/.fuelsequencer/config/app.toml
```

## Create service
```
sudo tee  > /dev/null <<EOF2
[Unit]
Description=fuelsequencerd Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which fuelsequencerd) start
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
sudo systemctl enable fuelsequencerd
sudo systemctl restart fuelsequencerd && sudo journalctl -u fuelsequencerd -f -o cat
```
