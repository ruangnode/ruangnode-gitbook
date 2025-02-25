
# Services

<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/$PROJECT_NAME.png" alt=""><figcaption></figcaption></figure>

AtomOne is a community-driven, constitutionally governed blockchain designed to prioritize security, decentralization, and innovation within the Cosmos ecosystem. Serving as a minimal fork of the Cosmos Hub, it supports IBC and ICS for scalable interchain solutions.

**Chain ID**: $CHAIN_ID | **BIN_VERSION**: $BIN_VERSION  | **Wasm**: $WASM

[Website]($WEBSITE) | [Discord]($DISCORD) | [Twitter]($TWITTER)

## Restake

[Restake with ruangnode]($RESTAKE)
## Chain explorer
[https://explorer.ruangnode.com]($EXPLORER)

## Public endpoints

* api: [https://m-$PROJECT_NAME-api.ruangnode.com/](https://m-$PROJECT_NAME-api.ruangnode.com/)
* rpc: [https://m-$PROJECT_NAME-rpc.ruangnode.com/](https://m-$PROJECT_NAME-rpc.ruangnode.com/)
* grpc: m-$PROJECT_NAME-grpc.ruangnode.com:${CUSTOM_PORT}090

## Peering

**peers**

```
$PEER
```

**seed-node**

```
$SEED
```

**addrbook**
```
wget -O \$HOME/.$PROJECT_NAME/config/addrbook.json https://server-1.ruangnode.com/mainnet/$PROJECT_NAME/addrbook.json
```

**live-peers** (8)
```
peers="$LIVE_PEER"
```
