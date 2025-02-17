## Manual Upgrade
```
cd $HOME
wget https://server-1.ruangnode.com/mainnet/atomone/atomoned
sudo mv $HOME/atomoned $(which atomoned)
sudo systemctl restart atomoned && sudo journalctl -u atomoned -f
```
