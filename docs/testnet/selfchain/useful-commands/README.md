<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/selfchain.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
selfchaind keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
selfchaind keys list
```


## Create Validator

Check your wallet balance:
```
selfchaind query bank balances wallet
```

To create your validator, run the command below:
```
selfchaind tx staking create-validator \
  --amount 1000000uself \
  --from=wallet \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey $(selfchaind tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=selfchain-testnet \
  --gas-adjustment 1.4 \
  --gas auto \
  --gas-prices 0.005uself
```

## Staking, Delegation, and Rewards
Delegate stake:
```
selfchaind tx staking delegate $VALOPER_ADDRESS 1000000uself --from=wallet --chain-id=selfchain-testnet --gas=auto
```

Redelegate stake:
```
selfchaind tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000uself --from=wallet --chain-id=selfchain-testnet --gas=auto
```

Withdraw all rewards:
```
selfchaind tx distribution withdraw-all-rewards --from=wallet --chain-id=selfchain-testnet --gas=auto
```

Withdraw rewards with commission:
```
selfchaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=selfchain-testnet
```

## Validator Management
Edit validator:
```
selfchaind tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=selfchain-testnet \
  --from=wallet
```

Unjail validator:
```
selfchaind tx slashing unjail --from=wallet --chain-id=selfchain-testnet --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u selfchaind -fo cat
```

Check service status
```
sudo systemctl status selfchaind
```

Node info
```
selfchaind status 2>&1 | jq
```

Start service
```
sudo systemctl start selfchaind
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop selfchaind
```

Enable Service
```
sudo systemctl enable selfchaind
```

Restart service
```
sudo systemctl restart selfchaind
```

Disable Service
```
sudo systemctl disable selfchaind
```
