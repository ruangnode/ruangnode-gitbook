<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>
## Manual Upgrade
```
cd $HOME
wget https://server-1.ruangnode.com/testnet/atomone/atomoned
sudo mv $HOME/atomoned $(which atomoned)
sudo systemctl restart atomoned && sudo journalctl -u atomoned -f
```
