<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/crossfid.png" alt=""><figcaption></figcaption></figure>

## Manual Upgrade

```
cd $HOME
wget https://server-1.ruangnode.com/mainnet/crossfid/crossfid
sudo mv $HOME/crossfid $(which crossfid)
sudo systemctl restart crossfid && sudo journalctl -u crossfid -f
```
