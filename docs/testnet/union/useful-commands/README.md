<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/union.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
uniond keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
uniond keys list
```


## Create Validator

Check your wallet balance:
```
uniond query bank balances wallet
```

To create your validator, run the command below:
```
uniond tx staking create-validator \
  --amount 1000000muno \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(uniond tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=union-testnet-9 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001muno
```

## Staking, Delegation, and Rewards
Delegate stake:
```
uniond tx staking delegate $VALOPER_ADDRESS 1000000muno --from=wallet --chain-id=union-testnet-9 --gas=auto
```

Redelegate stake:
```
uniond tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000muno --from=wallet --chain-id=union-testnet-9 --gas=auto
```

Withdraw all rewards:
```
uniond tx distribution withdraw-all-rewards --from=wallet --chain-id=union-testnet-9 --gas=auto
```

Withdraw rewards with commission:
```
uniond tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=union-testnet-9
```

## Validator Management
Edit validator:
```
uniond tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=union-testnet-9 \
  --from=wallet
```

Unjail validator:
```
uniond tx slashing unjail --from=wallet --chain-id=union-testnet-9 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u uniond -fo cat
```

Check service status
```
sudo systemctl status uniond
```

Node info
```
uniond status 2>&1 | jq
```

Start service
```
sudo systemctl start uniond
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop uniond
```

Enable Service
```
sudo systemctl enable uniond
```

Restart service
```
sudo systemctl restart uniond
```

Disable Service
```
sudo systemctl disable uniond
```
