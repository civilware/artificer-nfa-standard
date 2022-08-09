# Minting - Artificer NFT Market Standard <!-- omit in toc -->
Copyright 2022 Civilware. All rights reserved.<br>
dero1qy0khp9s9yw2h0eu20xmy9lth3zp5cacmx3rwt6k45l568d2mmcf6qgcsevzx

## Table of Contents <!-- omit in toc -->

- [Deploying With Artificer](#deploying-with-artificer)
- [Signing File Contents](#signing-file-contents)
- [Modifying The Template](#modifying-the-template)
    - [artificerFee](#artificerfee)
    - [royalty](#royalty)
    - [ownerCanUpdate](#ownercanupdate)
    - [nameHdr](#namehdr)
    - [descrHdr](#descrhdr)
    - [typeHdr](#typehdr)
    - [iconURLHdr](#iconurlhdr)
    - [tagsHdr](#tagshdr)
    - [fileCheckC](#filecheckc)
    - [fileCheckS](#filechecks)
    - [fileURL](#fileurl)
    - [fileSignURL](#filesignurl)
    - [coverURL](#coverurl)
    - [collection](#collection)
- [Deploying](#deploying)

## Deploying With Artificer
** Coming Soon **

## Signing File Contents
* [filesign](https://github.com/deroproject/derohe/blob/main/cmd/dero-wallet-cli/prompt.go#L139) is usable from within the CLI only as of this writeup. It will soon be added within [Engram](https://github.com/DEROFDN/Engram) for use as well as through Artificer. You do not need to be connected to a daemon in order to use this capability, simply opening your wallet.db file (or recovering from seed) using the CLI [binaries](https://github.com/deroproject/derohe/releases/latest). 
  * Copy your files to sign to the same directory as your wallet binary
  * Once inside your wallet, simply type ```filesign``` .
  * You'll be asked for the name of the file to sign. Provide the full filepath, such as ```myawesomenfa.txt```
  * A 'filename'.sign file will be generated. You will store this somewhere accessible and the URL to it is what you use for the [fileSignURL](#filesignurl) portion of the template section below.

* [fileverify](https://github.com/deroproject/derohe/blob/main/cmd/dero-wallet-cli/prompt.go#L161) is usable from within the CLI only as of this writeup. It will soon be added within [Engram](https://github.com/DEROFDN/Engram) for use as well as through Artificer. You do not need to be connected to a daemon in order to use this capability, simply opening your wallet.db file (or recovering from seed) using the CLI [binaries](https://github.com/deroproject/derohe/releases/latest). This is what Artificer and other dApps/programs can use to re-hydrate your original file from the .sign file generated above. 
  * Copy your file.sign file into the same directory as your wallet binary
  * Once inside your wallet, simply type ```fileverify``` .
  * You'll be asked for the name of the .sign file. Provide the full filepath, such as ```myawesomenfa.txt.sign```
  * A 'filename' file will be generated, matching the original. You will store this somewhere accessible and the URL to it is what you use for the [fileURL](#fileurl) portion of the template section below. 
    * NOTE: It is not required to perform this rehydration if you have the original already, however it is good practice and a nice validation that the .sign file is 100% valid.

## Modifying The Template
* Pull down the [ART-NFA-MS1.bas](ART-NFA-MS1.bas) contract template and store it locally. You will be modifying the values within the InitializePrivate() function to make the asset your own.
* As described in [Initialization](ART-NFA-MS1.md#initialization) , update the values to reflect your unique content and values to reflect it.

#### artificerFee
* A Uint64 value defining a percent donation sent to the Civilware developers. This is 0 by default. Calculations including this percent occur on the finalized sale/auction of an asset.

#### royalty
* A Uint64 value defining a percent fee sent to the creator of the asset between 0 and 100. This is 0 by default. Calculations including this percent occur on the finalized sale/auction of an asset.

#### ownerCanUpdate
* A Uint64 value that defines whether only a creator (0) can update tags/urls or an owner and/or a creator (1) can update tags/urls.

#### nameHdr
* A string value defining the name of the NFA, following the [headers](../Headers/Headers.md#headers) variable standard.

#### descrHdr
* A string value defining the description of the NFA, following the [headers](../Headers/Headers.md#headers) variable standard.

#### typeHdr
* A string value defining the type of the NFA, following the [headers](../Headers/Headers.md#headers) variable standard.

#### iconURLHdr
* A string value defining the url for the icon representing the NFA, following the [headers](../Headers/Headers.md#headers) variable standard. This should be of size 100x100.

#### tagsHdr
* A string value defining the tags used in reference to this asset, following the [headers](../Headers/Headers.md#headers) variable standard.

#### fileCheckC
* A string value defining the file signature 'C' header within the .sign file generated from the original asset by the owner. Use the filesign function and supply the original file prior to deploying this asset.

#### fileCheckS
* A string value defining the file signature 'S' header within the .sign file generated from the original asset by the owner. Use the filesign function and supply the original file prior to deploying this asset.

#### fileURL
* A string value defining the url for the file which this NFA is minting. This can be hosted anywhere the creator desires and is updateable by creator and/or owner depending on the setting of ownerCanUpdate (0/1).

#### fileSignURL
* A string value defining the url for the file signature which this NFA is minting. This signature is generated through the filesign capabilities of the DERO wallet. fileCheckC and fileCheckS are populated from the original filesign and capable of being re-validated against this data for future dApp integrations.

#### coverURL
* A string value defining the url for the cover representing the NFA. This is generally a "smaller" version of the  original or some variation that can be loaded quickly, but slightly larger than the icon url.

#### collection
* A string value defining if the NFA is a part of a collection or not. It is only capable of being set on installation.

## Deploying
Run the following from the same directory as the modified ART-NFA-MS1.bas file from the previous [section](ART-NFA-MS1-Minting.md#modifying-the-template). Ensure your wallet is running with --rpc-server if on [CLI](https://github.com/deroproject/derohe/releases/latest) or temporarily without authenticator on [Engram](https://github.com/DEROFDN/Engram/releases/latest).
```
curl --request POST --data-binary @ART-NFA-MS1.bas http://127.0.0.1:40403/install_sc
```