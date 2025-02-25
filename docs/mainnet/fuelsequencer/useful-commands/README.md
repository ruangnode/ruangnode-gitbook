<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/fuel.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
fuelsequencerd keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
fuelsequencerd keys list
```


## Create Validator

Check your wallet balance:
```
fuelsequencerd query bank balances wallet
```

To create your validator, run the command below:
```
fuelsequencerd tx staking create-validator \
  --amount 1000000fuel \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(fuelsequencerd tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=seq-mainnet-1 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001fuel
```

## Staking, Delegation, and Rewards
Delegate stake:
```
fuelsequencerd tx staking delegate $VALOPER_ADDRESS 1000000fuel --from=wallet --chain-id=seq-mainnet-1 --gas=auto
```

Redelegate stake:
```
fuelsequencerd tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000fuel --from=wallet --chain-id=seq-mainnet-1 --gas=auto
```

Withdraw all rewards:
```
fuelsequencerd tx distribution withdraw-all-rewards --from=wallet --chain-id=seq-mainnet-1 --gas=auto
```

Withdraw rewards with commission:
```
fuelsequencerd tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=seq-mainnet-1
```

## Validator Management
Edit validator:
```
fuelsequencerd tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=seq-mainnet-1 \
  --from=wallet
```

Unjail validator:
```
fuelsequencerd tx slashing unjail --from=wallet --chain-id=seq-mainnet-1 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u fuelsequencerd -fo cat
```

Check service status
```
sudo systemctl status fuelsequencerd
```

Node info
```
fuelsequencerd status 2>&1 | jq
```

Start service
```
sudo systemctl start fuelsequencerd
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop fuelsequencerd
```

Enable Service
```
sudo systemctl enable fuelsequencerd
```

Restart service
```
sudo systemctl restart fuelsequencerd
```

Disable Service
```
sudo systemctl disable fuelsequencerd
```
