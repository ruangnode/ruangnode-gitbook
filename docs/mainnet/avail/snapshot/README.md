<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/avail.png" alt=""><figcaption></figcaption></figure>

## Snapshot
To restore a snapshot, follow the steps below:

```
sudo systemctl stop availd.service
```
## Reset the data and download latest snapshot
```
# Remove data

rm -r $HOME/.avail/data/chains/avail_da_mainnet/paritydb/

# Download latest snapshot

curl -L https://server-1.ruangnode.com/mainnet/avail/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.avail/data/chains/avail_da_mainnet
```
## Start the service and check the log
```
sudo systemctl start availd.service && journalctl -fu availd.service -o cat
```
