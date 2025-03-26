<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/crossfid.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop crossfid
cp $HOME/.crossfid/data/priv_validator_state.json $HOME/.crossfid/priv_validator_state.json.backup
rm -rf $HOME/.crossfid/data

SNAP_NAME=$(curl -s "" | grep -oE "crossfid_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
curl ${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.crossfid
mv $HOME/.crossfid/priv_validator_state.json.backup $HOME/.crossfid/data/priv_validator_state.json
sudo systemctl restart crossfid && sudo journalctl -u crossfid -f
```

