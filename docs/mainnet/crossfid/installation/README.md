---
description: Setting up your validator node has never been so easy. Get your validator running in minutes by following step by step instructions.
---
# Installation
<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/crossfid.png" alt=""><figcaption></figcaption></figure>
**Chain ID**: crossfi-mainnet-1 | **Latest Version Tag**: v0.3.0  | **Custom Port**: 10

```
echo "export NODENAME=Yournodename" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=crossfi-mainnet-1" >> $HOME/.bash_profile
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
wget "https://golang.org/dl/go.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go.linux-amd64.tar.gz"
rm "go.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

## Download and build binaries
```
cd $HOME
wget https://server-1.ruangnode.com/mainnet/crossfid/crossfid
chmod +x crossfid
mv crossfid $HOME/go/bin/crossfid
```

## Config & Init app
```
rm -rf testnet ~/.mineplex-chain
git clone https://github.com/crossfichain/mainnet.git
mv $HOME/mainnet/ $HOME/.crossfid/
sed -i '99,114 s/^\( *enable =\).*/\1 "false"/' $HOME/.crossfid/config/config.toml
```

## Download configuration
```
cd $HOME
wget -O $HOME/.crossfid/config/genesis.json https://server-1.ruangnode.com/mainnet/crossfid/genesis.json
wget -O $HOME/.crossfid/config/addrbook.json https://server-1.ruangnode.com/mainnet/crossfid/addrbook.json
```

```
SEEDS=""
PEERS="641157ecbfec8e0ec37ca4c411c1208ca1327154@crossfi-mainnet-peer.itrocket.net:11656,d996012096cfef860bf24543740d58da45e5b194@37.27.183.62:26656,b0c009c88f88dab8f5db00c351c3b8f43acdb800@116.202.160.22:26656,529e0d1bce51ea207488e6de7c90d952ea40c264@[2a03:cfc0:8000:13::b910:277f]:13256,f8cbc62fb487ae825edf79c580206d0e34ee9f51@5.161.229.160:26656,ea465c58b6de1553c61bc528cc2675956b2c52f5@135.181.152.201:26656,f27eff68f2f3542a317bad66fdf9f1cc93a80dc1@49.13.76.170:26656,4244a2159876c4a72cf1d6117bf2003bece8c08a@65.21.196.57:37656,bcdf72c1be64fec2b96c288bce7c776a950b82da@65.21.146.240:26656,f5d2b1a6ab68ac9357366afe424564ab42a9d444@185.107.82.171:26656,773a284b788b76e3b4a8f228e85178b00fe8af95@65.108.151.146:26656,aa95f123e72a8ee3d75893aca4040e51052ac104@135.181.57.156:26056,6ff27ca8b8b08705780df8575891d669b0286cbe@37.27.112.208:22656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
       -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.crossfid/config/config.toml
```

```
# set custom ports in app.toml
sed -i.bak -e "s%:1317%:$\{CUSTOM_PORT}317%g;
s%:8080%:${CUSTOM_PORT}080%g;
s%:9090%:${CUSTOM_PORT}090%g;
s%:9091%:${CUSTOM_PORT}091%g;
s%:8545%:${CUSTOM_PORT}545%g;
s%:8546%:${CUSTOM_PORT}546%g;
s%:6065%:${CUSTOM_PORT}065%g" $HOME/.crossfid/config/app.toml
```

```
# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${CUSTOM_PORT}658%g;
s%:26657%:${CUSTOM_PORT}657%g;
s%:6060%:${CUSTOM_PORT}060%g;
s%:26656%:${CUSTOM_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CUSTOM_PORT}656\"%;
s%:26660%:${CUSTOM_PORT}660%g" $HOME/.crossfid/config/config.toml
```

## Disable indexing
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.crossfid/config/config.toml
```

## Config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.crossfid/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.crossfid/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.crossfid/config/app.toml
```

## Set minimum gas price
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "10000000000000mpx"|g' $HOME/.crossfid/config/app.toml
```

## Create service
```
sudo tee  > /dev/null <<EOF2
[Unit]
Description=crossfid Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which crossfid) start --home $HOME/.crossfid
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
sudo systemctl enable crossfid
sudo systemctl restart crossfid && sudo journalctl -u crossfid -f -o cat
```
