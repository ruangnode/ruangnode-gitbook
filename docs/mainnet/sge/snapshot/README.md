<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/sge.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop sged
cp $HOME/.sge/data/priv_validator_state.json $HOME/.sge/priv_validator_state.json.backup
rm -rf $HOME/.sge/data

SNAP_NAME=$(curl -s "" | grep -oE "atomone_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
curl ${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.sge
mv $HOME/.sge/priv_validator_state.json.backup $HOME/.sge/data/priv_validator_state.json
sudo systemctl restart sged && sudo journalctl -u sged -f
```

