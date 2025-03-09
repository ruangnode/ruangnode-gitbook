<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/crossfid.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
crossfid keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
crossfid keys list
```


## Create Validator

Check your wallet balance:
```
crossfid query bank balances wallet
```

To create your validator, run the command below:
```
crossfid tx staking create-validator \
  --amount 1000000mpx \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(crossfid tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=crossfi-mainnet-1 \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.001mpx
```

## Staking, Delegation, and Rewards
Delegate stake:
```
crossfid tx staking delegate $VALOPER_ADDRESS 1000000mpx --from=wallet --chain-id=crossfi-mainnet-1 --gas=auto
```

Redelegate stake:
```
crossfid tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000mpx --from=wallet --chain-id=crossfi-mainnet-1 --gas=auto
```

Withdraw all rewards:
```
crossfid tx distribution withdraw-all-rewards --from=wallet --chain-id=crossfi-mainnet-1 --gas=auto
```

Withdraw rewards with commission:
```
crossfid tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=crossfi-mainnet-1
```

## Validator Management
Edit validator:
```
crossfid tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=crossfi-mainnet-1 \
  --from=wallet
```

Unjail validator:
```
crossfid tx slashing unjail --from=wallet --chain-id=crossfi-mainnet-1 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u crossfid -fo cat
```

Check service status
```
sudo systemctl status crossfid
```

Node info
```
crossfid status 2>&1 | jq
```

Start service
```
sudo systemctl start crossfid
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop crossfid
```

Enable Service
```
sudo systemctl enable crossfid
```

Restart service
```
sudo systemctl restart crossfid
```

Disable Service
```
sudo systemctl disable crossfid
```
