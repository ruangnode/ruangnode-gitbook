<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop atomoned
cp $HOME/.atomone/data/priv_validator_state.json $HOME/.atomone/priv_validator_state.json.backup
rm -rf $HOME/.atomone/data
SNAP_NAME=$(curl -s "https://server-1.ruangnode.com/mainnet/atomone/" | grep -oE "atomone_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
curl https://server-1.ruangnode.com/mainnet/atomone/${SNAP_NAME} | lz4 -dc - | tar -xf - -C $HOME/.atomone
mv $HOME/.atomone/priv_validator_state.json.backup $HOME/.atomone/data/priv_validator_state.json
sudo systemctl restart atomoned && sudo journalctl -u atomoned -f
```

