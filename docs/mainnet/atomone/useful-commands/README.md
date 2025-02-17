<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:

```
atomoned keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
atomoned keys list
```


## Create Validator

Check your wallet balance:
```
atomoned query bank balances wallet
```

To create your validator, run the command below:
```
atomoned tx staking create-validator \
  --amount 1000000atone \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(atomoned tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=atomone-1 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001atone
```

## Staking, Delegation, and Rewards
Delegate stake:
```
atomoned tx staking delegate $VALOPER_ADDRESS 1000000atone --from=wallet --chain-id=atomone-1 --gas=auto
```

Redelegate stake:
```
atomoned tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000atone --from=wallet --chain-id=atomone-1 --gas=auto
```

Withdraw all rewards:
```
atomoned tx distribution withdraw-all-rewards --from=wallet --chain-id=atomone-1 --gas=auto
```

Withdraw rewards with commission:
```
atomoned tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=atomone-1
```

## Validator Management
Edit validator:
```
atomoned tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=atomone-1 \
  --from=wallet
```

Unjail validator:
```
atomoned tx slashing unjail --from=wallet --chain-id=atomone-1 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u atomoned -fo cat
```

Check service status
```
sudo systemctl status atomoned
```

Node info
```
atomoned status 2>&1 | jq
```

Start service
```
sudo systemctl start atomoned
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop atomoned
```

Enable Service
```
sudo systemctl enable atomoned
```

Restart service
```
sudo systemctl restart atomoned
```

Disable Service
```
sudo systemctl disable atomoned
```
