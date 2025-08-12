## Snapshot

Stop service & backup state:
```
sudo systemctl stop dorad
cp $HOME/.dora/data/priv_validator_state.json $HOME/.dora/priv_validator_state.json.backup || true
rm -rf $HOME/.dora/data
```

Restore latest snapshot:
```
SNAP_NAME=$(curl -s "https://server-1.ruangnode.com/mainnet/doravota/" | grep -oE "doravota_[0-9-]+_[0-9]+_snap\.tar\.lz4" | sort -t_ -k3 -nr | head -n 1)
echo "Using snapshot: ${SNAP_NAME}"
curl -L "https://server-1.ruangnode.com/mainnet/doravota/${SNAP_NAME}" | lz4 -dc - | tar -xf - -C $HOME/.dora
mv $HOME/.dora/priv_validator_state.json.backup $HOME/.dora/data/priv_validator_state.json
sudo systemctl restart dorad && sudo journalctl -u dorad -f -o cat
```
