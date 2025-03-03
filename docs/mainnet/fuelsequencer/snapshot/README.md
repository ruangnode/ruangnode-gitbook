<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/fuelsequencer.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop fuelsequencerd
cp $HOME/.fuelsequencer/data/priv_validator_state.json $HOME/.fuelsequencer/priv_validator_state.json.backup
rm -rf $HOME/.fuelsequencer/data

SNAP_NAME=$(curl -s "https://server-1.ruangnode.com/mainnet/fuelsequencer/" | grep -oE "fuel_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
curl https://server-1.ruangnode.com/mainnet/fuelsequencer/${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.fuelsequencer
mv $HOME/.fuelsequencer/priv_validator_state.json.backup $HOME/.fuelsequencer/data/priv_validator_state.json
sudo systemctl restart fuelsequencerd && sudo journalctl -u fuelsequencerd -f
```

