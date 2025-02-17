# Upgrade

<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>

**Chain ID**: atomone-1 | **Latest Version Tag**: v1.0.1  | **Custom Port**: 10
## Manual Upgrade
```
cd $HOME
wget https://server-1.ruangnode.com/mainnet/atomone/atomoned
sudo mv $HOME/atomoned $(which atomoned)
sudo systemctl restart atomoned && sudo journalctl -u atomoned -f
```
