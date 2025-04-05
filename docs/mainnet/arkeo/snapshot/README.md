<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/arkeo.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop arkeod
cp $HOME/.arkeod/data/priv_validator_state.json $HOME/.arkeod/priv_validator_state.json.backup
rm -rf $HOME/.arkeod/data

SNAP_NAME=$(curl -s "" | grep -oE "atomone_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
curl ${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.arkeod
mv $HOME/.arkeod/priv_validator_state.json.backup $HOME/.arkeod/data/priv_validator_state.json
sudo systemctl restart arkeod && sudo journalctl -u arkeod -f
```

