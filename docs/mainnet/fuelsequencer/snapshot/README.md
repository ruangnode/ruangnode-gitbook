<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/fuel.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop fuelsequencerd
cp $HOME/.fuelsequencer/data/priv_validator_state.json $HOME/.fuelsequencer/priv_validator_state.json.backup
rm -rf $HOME/.fuelsequencer/data
curl https://server-1.ruangnode.com/mainnet/fuelsequencer/fuelsequencer-snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.fuelsequencer
mv $HOME/.fuelsequencer/priv_validator_state.json.backup $HOME/.fuelsequencer/data/priv_validator_state.json
sudo systemctl restart fuelsequencerd && sudo journalctl -u fuelsequencerd -f
```

