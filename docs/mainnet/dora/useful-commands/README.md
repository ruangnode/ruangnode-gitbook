## Wallet
```
dorad keys add wallet
dorad keys add wallet --recover
dorad keys list
```

## Create Validator
```
dorad tx staking create-validator \
  --amount 1000000peaka \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(dorad tendermint show-validator --home $HOME/.dora) \
  --moniker "$NODENAME" \
  --chain-id=vota-ash \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001peaka
```

## Staking / Rewards
```
dorad tx staking delegate $VALOPER_ADDRESS 1000000peaka --from=wallet --chain-id=vota-ash --gas=auto
dorad tx staking redelegate <srcVal> <dstVal> 1000000peaka --from=wallet --chain-id=vota-ash --gas=auto
dorad tx distribution withdraw-all-rewards --from=wallet --chain-id=vota-ash --gas=auto
dorad tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=vota-ash
```

## Service mgmt
```
sudo journalctl -u dorad -fo cat
sudo systemctl status dorad
dorad status 2>&1 | jq
sudo systemctl start dorad
sudo systemctl daemon-reload
sudo systemctl stop dorad
sudo systemctl enable dorad
sudo systemctl restart dorad
sudo systemctl disable dorad
```
