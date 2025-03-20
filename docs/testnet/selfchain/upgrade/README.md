<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/selfchain.png" alt=""><figcaption></figcaption></figure>

## Manual Upgrade

```
cd $HOME
wget https://server-1.ruangnode.com/testnet/selfchain/selfchaind-v2.0.0
mv selfchaind-v2.0.0 selfchaind
chmod +x selfchaind
mv $HOME/selfchaind $(which selfchaind)
sudo systemctl restart selfchaind && sudo journalctl -u selfchaind -f
```
