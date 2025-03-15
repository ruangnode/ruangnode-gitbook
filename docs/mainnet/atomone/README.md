# Services

<figure><img src="https://raw.githubusercontent.com/ruangnode/cosmos-images/main/logos/atomone.png" alt=""><figcaption></figcaption></figure>

AtomOne is a community-driven, constitutionally governed blockchain designed to prioritize security, decentralization, and innovation within the Cosmos ecosystem. Serving as a minimal fork of the Cosmos Hub, it supports IBC and ICS for scalable interchain solutions.

**Chain ID**: atomone-1 | **v1.1.2**:  | **Wasm**: OFF

[Website](https://atom.one) | [Discord](https://discord.gg/Fd7W5zyd63) | [Twitter](https://x.com/_atomone)

## Restake

[Restake with ruangnode](https://restake.app/atomone/atonevaloper1phwcgsqpscthtuqwz7yfqqkpew2y0hscqygk9m)
## Chain explorer
[https://explorer.ruangnode.com/atomone-mainnet](https://explorer.ruangnode.com/atomone-mainnet)

## Public endpoints

* api: [https://m-atomone-api.ruangnode.com/](https://m-atomone-api.ruangnode.com/)
* rpc: [https://m-atomone-rpc.ruangnode.com/](https://m-atomone-rpc.ruangnode.com/)
* grpc: m-atomone-grpc.ruangnode.com:62090

## Peering

**peers**

```
991aa9bac0adda31b27ae84f0997c96691f6c2e7@95.217.62.179:12956
```

**seed-node**

```
0ca5a9671b989addb8c5b4f8c9a4af9b0206f5c4@37.27.52.25:62656
```

**addrbook**
```bash
curl -Ls https://server-1.ruangnode.com/mainnet/atomone/addrbook.json > $HOME/.atomone/config/addrbook.json
```

**live-peers** (8)
```
peers="ed0e36c57122184ab05b6c635b2f2adf592bfa0c@atomone-mainnet-peer.itrocket.net:61657,5d913650738a081aa02631a7f108dc7812330f0b@37.27.129.24:13656,755b3c1ecedb05ff08929da3b17174230a009182@138.201.200.188:29956,752bb5f1c914c5294e0844ddc908548115c1052c@65.108.236.5:14556,d1a6a7cb6e35bfb0cad31435eaaa9d867bcc9d23@202.61.243.56:29956,35e145a522e2d81862fdd05fa68296ed8e197d3a@37.27.71.199:23656,8391dab9a9ece4e3f80e06512bdd1a84af5f257f@95.217.36.103:14556,b56c9a6b51114cd7ea71e8010e12ff9adbc17e1c@195.3.223.73:36656,b1cd93aaa6ce46e536893c3d0ae064134e70849f@65.21.79.120:26656,37201c92625df2814a55129f73f10ab6aa2edc35@185.16.39.137:27396,cabab04a4a680c768529a36b4d8d62761050ba26@65.109.68.170:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.atomone/config/config.toml
```
