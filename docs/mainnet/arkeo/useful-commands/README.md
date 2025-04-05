<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/arkeo.png" alt=""><figcaption></figcaption></figure>

## Create Wallet
To create a new wallet, use the command below. Don’t forget to save the mnemonic:
```
arkeod keys add wallet
```

To recover your wallet using a seed phrase:
```
 keys add wallet --recover
```

Show your wallet list:
```
arkeod keys list
```


## Create Validator

Check your wallet balance:
```
arkeod query bank balances wallet
```
pubkey
```
arkeod comet show-validator
```

To create your validator, run the command below:
```
nano $HOME/.arkeo/validator.json

{
  "pubkey": <pubkey>,
  "amount": "100000000uarkeo",
  "moniker": "<moniker>",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.1",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.1",
  "min-self-delegation": "1"
}

arkeod tx staking create-validator $HOME/.arkeo/validator.json \
    --from=<name_wallet> \
    --chain-id=arkeo-main-v1 \
    --fees=5000uarkeo -y
```

## Staking, Delegation, and Rewards
Delegate stake:
```
arkeod tx staking delegate $VALOPER_ADDRESS 1000000uarkeo --from=wallet --chain-id=arkeo-main-v1 --gas=auto
```

Redelegate stake:
```
arkeod tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 1000000uarkeo --from=wallet --chain-id=arkeo-main-v1 --gas=auto
```

Withdraw all rewards:
```
arkeod tx distribution withdraw-all-rewards --from=wallet --chain-id=arkeo-main-v1 --gas=auto
```

Withdraw rewards with commission:
```
arkeod tx distribution withdraw-rewards $VALOPER_ADDRESS --from=wallet --commission --chain-id=arkeo-main-v1
```

## Validator Management
Edit validator:
```
arkeod tx staking edit-validator \
  --moniker=$MONIKER \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=arkeo-main-v1 \
  --from=wallet
```

Unjail validator:
```
arkeod tx slashing unjail --from=wallet --chain-id=arkeo-main-v1 --gas=auto
```

## Service Management
Check logs
```
sudo journalctl -u arkeod -fo cat
```

Check service status
```
sudo systemctl status arkeod
```

Node info
```
arkeod status 2>&1 | jq
```

Start service
```
sudo systemctl start arkeod
```

Reload services
```
sudo systemctl daemon-reload
```

Stop service
```
sudo systemctl stop arkeod
```

Enable Service
```
sudo systemctl enable arkeod
```

Restart service
```
sudo systemctl restart arkeod
```

Disable Service
```
sudo systemctl disable arkeod
```
