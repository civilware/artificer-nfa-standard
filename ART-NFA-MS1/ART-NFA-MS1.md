# ART-NFA-MS1 - Artificer NFT Market Standard <!-- omit in toc -->
Copyright 2022 Civilware. All rights reserved.<br>
dero1qy0khp9s9yw2h0eu20xmy9lth3zp5cacmx3rwt6k45l568d2mmcf6qgcsevzx

## Introduction <!-- omit in toc -->

### What is ART-NFA-MS1<!-- omit in toc -->

The ART-NFA-MS1 (Artificer NFT Market Standard) introduces a standard for Non-Fungible Assets with a built-in trustless marketplace. An ART-NFA-MS1 Asset can be exchanged within the contract through two methods, auction and sale. Sales are started with a defined price and allows for BuyItNow() actions to be taken to sell the asset. Auctions are started with a defined bid price which will increase as more bids are placed. A ART-NFA-MS1 Asset can also be exchanged privately through native DERO transactions. Assets only ever reside within the balance of the contract in the event of a sale or auction.

## Table of Contents <!-- omit in toc -->
- [Functionality](#functionality)
- [Functions](#functions)
- [Initialization](#initialization)
- [Contract Template](#contract-template)
- [Minting](#minting)
- [Changelog](#changelog)
- [Utilization](#utilization)
  - [Install_SC](#install_sc)
  - [ClaimOwnership()](#claimownership)
  - [Update()](#update)
  - [Start()](#start)
    - [Sale](#sale)
    - [Auction](#auction)
  - [BuyItNow()](#buyitnow)
  - [Bid()](#bid)
  - [CloseListing()](#closelisting)
  - [CancelListing()](#cancellisting)

## Functionality

Example functionalities ART-NFA-MS1 provides:

* Issue an asset which correlates to a given fileurl and coverurl for display in dApps, like Artificer (coming soon).
* Assets could be anything from Images, Video, Audio, Application etc.
* ART-NFA-MS1 leverages DERO filesign capabilities to validate authenticity of the reflected asset files.
* Start a sale or auction with capabilities of custom creator royalty, developer donation or charity donation.
  * Charity donations can be defined custom at each start of a sale/auction while Artificer donations and creator royalties are defined on asset creation.
* Transfer asset privately through normal DERO transactions and claim ownership to be reflected in the contract.

## Functions

```go
Function ClaimOwnership() Uint64
Function Update(iconURL String, coverURL String, fileURL String, fileSignURL String, tags String) Uint64
Function Start(listType String, duration Uint64, startPrice Uint64, charityDonateAddr String, charityDonatePerc Uint64) Uint64
Function BuyItNow() Uint64
Function Bid() Uint64
Function CloseListing() Uint64
Function CancelListing() Uint64
```

* [Functions](ART-NFA-MS1-Functions.md)

## Initialization

When deploying the standard with Artificer (coming soon), metadata values are input for you and no action required. If you are deploying this standard manually, update the metadata values outlined below. For header values you can refer to the [Headers](../Headers/Headers.md) standard.

```go
Function InitializePrivate() Uint64
    ...
    300 STORE("artificerFee", 0)    // This is a donation back to the Civilware developers. It can range between 0 and 100, taking into account remaining below 100 with artificerFee + royalty. It is 0 by default.
    310 STORE("royalty", 0)   // This defines the royalty that is paid back to the original creator of the asset. It can range between 0 and 100, taking into account remaining below 100 with artificerFee + royalty. It is 0 by default.
    320 STORE("ownerCanUpdate", 0)  // This defines whether only a creator (0) can update tags/urls or an owner and/or a creator (1) can update tags/urls.
    330 STORE("nameHdr", "<nameHdr>") // This defines the name of the NFA, following the headers variable standard.
    340 STORE("descrHdr", "<descrHdr>") // This defines the description of the NFA, following the headers variable standard.
    350 STORE("typeHdr", "<typeHdr>") // This defines the type of the NFA, following the headers variable standard.
    360 STORE("iconURLHdr", "<iconURLHdr>") // This defines the url for the icon representing the NFA, following the headers variable standard. This should be of size 100x100.
    370 STORE("tagsHdr", "<tagsHdr>") // This defines the tags used in reference to this asset, following the headers variable standard.
    400 STORE("fileCheckC", "<fileCheckC>") // This defines the file signature 'C' header within the .sign file generated from the original asset by the owner.
    410 STORE("fileCheckS", "<fileCheckS>") // This defines the file signature 'S' header within the .sign file generated from the original asset by the owner.
    420 STORE("fileURL", "<fileURL>") // This defines the url for the file which this NFA is minting. This can be hosted anywhere the creator desires and is updateable by creator and/or owner depending on setting of ownerCanUpdate (0/1).
    430 STORE("fileSignURL", "<fileSignURL>") // This defines the url for the file signature which this NFA is minting. This signature is generated through the filesign capabilities of the DERO wallet. fileCheckC and fileCheckS are populated from the original filesign and capable of being re-validated against this data for future dApp integrations.
    440 STORE("coverURL", "<coverURL>") // This defines the url for the cover representing the NFA. This is generally a "smaller" version of the original or some variation that can be loaded quickly, but slightly larger than the icon url.
    450 STORE("collection", "<collection>") // This defines if the NFA is a part of a collection or not. It is only capable of being set on installation.
    ...
End Function 
```

## Contract Template
* [ART-NFA-MS1](ART-NFA-MS1.bas)

## Minting
* [Minting](ART-NFA-MS1-Minting.md)

## Changelog
* [Changelog](Changelog.md)

## Utilization

It is in your best interest to always run a getgasestimate ahead of any direct curls below as well as use ringsize = 2. This will 1) reduce your errors and 2) ensure you are utilizing the proper amount for fees. Each portion below will be split with a getgasestimate example call first, and then the follow-up with the respective fees that were present in that exact scenario. It's up to you to modify the fees param to reflect the 'gasstorage' return of getgasestimate. Always use ringsize 2 to interact with the contract in order to not run the risk of unintended asset transfers.

Future dApp(s) will make this process/interaction seamless, however in the event you want to build your own or interact manually you can use the below curl commands to step through it. If you require some additional details for parameters etc. checkout the [Functions](ART-NFA-MS1-Functions.md) write-up.

### Install_SC
```
curl --request POST --data-binary @ART-NFA-MS1.bas http://127.0.0.1:40403/install_sc
```

### ClaimOwnership()
DISCLAIMER: If ringsize > 2 ..
```
- Assume you supplied appropriate 1 asset
- 1 asset sent back to current "owner" address.
```

GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"ClaimOwnership"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":155,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"ClaimOwnership"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```

### Update()
GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Update"},{"name":"iconURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-IC.png?raw=true"},{"name":"coverURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-CA.png?raw=true"},{"name":"fileURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-WP.png?raw=true"},{"name":"fileSignURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-WP.png.sign?raw=true"},{"name":"tags","datatype":"S","value":"#DERO"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":839,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Update"},{"name":"iconURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-IC.png?raw=true"},{"name":"coverURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-CA.png?raw=true"},{"name":"fileURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-WP.png?raw=true"},{"name":"fileSignURL","datatype":"S","value":"https://github.com/Azylem/AZYWP0001/blob/main/AZYWP0001-EnterTheMachine-WP.png.sign?raw=true"},{"name":"tags","datatype":"S","value":"#DERO"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```

### Start()
#### Sale
DISCLAIMER: If ringsize > 2 ..
```
- Assume you supplied appropriate 1 asset
- 1 asset sent back to current "owner" address.
```
DISCLAIMER: For unregistered charity addresses ..
```
- If you send an unregistered wallet address as the charity address, there is *currently* no internal SC catch for this. Please be sure if you are manually running that the wallet is registered. Use DERO.GetEncryptedBalance for testing if you choose to see if wallet is registered or not.
```

GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Start"},{"name":"listType","datatype":"S","value":"sale"},{"name":"duration","datatype":"U","value":24},{"name":"startPrice","datatype":"U","value":500},{"name":"charityDonateAddr","datatype":"S","value":"deto1qy0fwfmqlmnz6la2ey27qfsd9rt0mtjhset2say3rf77l0cu6luz7qg96n8x7"},{"name":"charityDonatePerc","datatype":"U","value":1},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":292,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Start"},{"name":"listType","datatype":"S","value":"sale"},{"name":"duration","datatype":"U","value":24},{"name":"startPrice","datatype":"U","value":500},{"name":"charityDonateAddr","datatype":"S","value":"deto1qy0fwfmqlmnz6la2ey27qfsd9rt0mtjhset2say3rf77l0cu6luz7qg96n8x7"},{"name":"charityDonatePerc","datatype":"U","value":1},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```
#### Auction
GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Start"},{"name":"listType","datatype":"S","value":"auction"},{"name":"duration","datatype":"U","value":24},{"name":"startPrice","datatype":"U","value":500},{"name":"charityDonateAddr","datatype":"S","value":"deto1qy0fwfmqlmnz6la2ey27qfsd9rt0mtjhset2say3rf77l0cu6luz7qg96n8x7"},{"name":"charityDonatePerc","datatype":"U","value":1},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":298,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Start"},{"name":"listType","datatype":"S","value":"auction"},{"name":"duration","datatype":"U","value":24},{"name":"startPrice","datatype":"U","value":500},{"name":"charityDonateAddr","datatype":"S","value":"deto1qy0fwfmqlmnz6la2ey27qfsd9rt0mtjhset2say3rf77l0cu6luz7qg96n8x7"},{"name":"charityDonatePerc","datatype":"U","value":1},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"scid":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4","amount":0,"burn":1}]}}'
```

### BuyItNow()
DISCLAIMER: If ringsize > 2 ..
```
- DEROVALUE() sent to be used to buy will just burn and not be 'visible'. Return 1 (get gas estimate should catch this err)
```

GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qysvnwagyh7aej4na5m4s57a4ntlfk5lwxgpvm7hs5twpl85u9r7uqgs30kks","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"BuyItNow"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"destination":"deto1qy0ehnqjpr0wxqnknyc66du2fsxyktppkr8m8e6jvplp954klfjz2qqdzcd8p","amount":0,"burn":500}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40411/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":200,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"BuyItNow"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"destination":"deto1qy0ehnqjpr0wxqnknyc66du2fsxyktppkr8m8e6jvplp954klfjz2qqdzcd8p","amount":0,"burn":500}]}}'
```

### Bid()
DISCLAIMER: If ringsize > 2 ..
```
- DEROVALUE() sent to be used to buy will just burn and not be 'visible'. Return 1 (get gas estimate should catch this err)
```

GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qysvnwagyh7aej4na5m4s57a4ntlfk5lwxgpvm7hs5twpl85u9r7uqgs30kks","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Bid"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"destination":"deto1qy0ehnqjpr0wxqnknyc66du2fsxyktppkr8m8e6jvplp954klfjz2qqdzcd8p","amount":0,"burn":500}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40411/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":200,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"Bid"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}],"transfers":[{"destination":"deto1qy0ehnqjpr0wxqnknyc66du2fsxyktppkr8m8e6jvplp954klfjz2qqdzcd8p","amount":0,"burn":500}]}}'
```

### CloseListing()
GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"CloseListing"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":225,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"CloseListing"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```

### CancelListing()
GetGasEstimate
```
curl -X POST http://127.0.0.1:40442/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"DERO.GetGasEstimate","params":{"signer":"deto1qy2fz6tx67resd2n4trf0gvtjwyh9y2j22m6e7wrshlrwcrw8kzjuqgs4vejf","ringsize":2,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"CancelListing"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```
Txn
```
curl -X POST http://127.0.0.1:40403/json_rpc -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":"0","method":"Transfer","params":{"ringsize":2,"fees":130,"sc_rpc":[{"name":"entrypoint","datatype":"S","value":"CancelListing"},{"name":"SC_ACTION","datatype":"U","value":0},{"name":"SC_ID","datatype":"H","value":"f73b9f4e222e86a579ba5a440e97eb7ef0712bc4ae93fbd19c0a1122b8c827b4"}]}}'
```