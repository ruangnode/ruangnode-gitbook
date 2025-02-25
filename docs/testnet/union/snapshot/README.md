<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/union.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop uniond
cp $HOME/.union/data/priv_validator_state.json $HOME/.union/priv_validator_state.json.backup
rm -rf $HOME/.union/data
curl https://server-1.ruangnode.com/testnet/union/union-snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.union
mv $HOME/.union/priv_validator_state.json.backup $HOME/.union/data/priv_validator_state.json
sudo systemctl restart uniond && sudo journalctl -u uniond -f
```

