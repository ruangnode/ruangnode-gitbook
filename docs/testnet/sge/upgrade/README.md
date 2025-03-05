<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/sge.png" alt=""><figcaption></figcaption></figure>

## Manual Upgrade

```
cd $HOME
wget https://server-1.ruangnode.com/testnet/sge/sged
sudo mv $HOME/sged $(which sged)
sudo systemctl restart sged && sudo journalctl -u sged -f
```
