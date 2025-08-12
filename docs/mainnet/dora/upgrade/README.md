## Manual Upgrade

```
cd $HOME
wget -O dorad https://server-1.ruangnode.com/mainnet/atomone/dorad
sudo mv $HOME/dorad $(which dorad)
sudo systemctl restart dorad && sudo journalctl -u dorad -f -o cat
```
