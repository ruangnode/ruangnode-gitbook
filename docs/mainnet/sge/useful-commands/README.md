<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/sge.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
sged keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
sged keys list
```


## Create Validator

Check your wallet balance:
```
sged query bank balances wallet
```

To create your validator, run the command below:
```
sged tx staking create-validator \
  --amount 1000000usge \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(sged tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=sgenet-1 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001usge
```

## Staking, Delegation, and Rewards
Delegate stake:
```
sged tx staking delegate $VALOPER_ADDRESS 1000000usge --from=wallet --chain-id=sgenet-1 --gas=auto
```

Redelegate stake:
```
sged tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000usge --from=wallet --chain-id=sgenet-1 --gas=auto
```

Withdraw all rewards:
```
sged tx distribution withdraw-all-rewards --from=wallet --chain-id=sgenet-1 --gas=auto
```

Withdraw rewards with commission:
```
sged tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=sgenet-1
```

## Validator Management
Edit validator:
```
sged tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=sgenet-1 \
  --from=wallet
```

Unjail validator:
```
sged tx slashing unjail --from=wallet --chain-id=sgenet-1 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u sged -fo cat
```

Check service status
```
sudo systemctl status sged
```

Node info
```
sged status 2>&1 | jq
```

Start service
```
sudo systemctl start sged
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop sged
```

Enable Service
```
sudo systemctl enable sged
```

Restart service
```
sudo systemctl restart sged
```

Disable Service
```
sudo systemctl disable sged
```
