# Functions - Artificer NFT Market Standard <!-- omit in toc -->
Copyright 2022 Civilware. All rights reserved.<br>
dero1qy0khp9s9yw2h0eu20xmy9lth3zp5cacmx3rwt6k45l568d2mmcf6qgcsevzx

## Table of Contents <!-- omit in toc -->

- [Function ClaimOwnership() Uint64](#function-claimownership-uint64)
- [Function Update(iconURL String, coverURL String, fileURL String, fileSignURL String, tags String) Uint64](#function-updateiconurl-string-coverurl-string-fileurl-string-filesignurl-string-tags-string-uint64)
- [Function Start(listType String, duration Uint64, startPrice Uint64, charityDonateAddr String, charityDonatePerc Uint64) Uint64](#function-startlisttype-string-duration-uint64-startprice-uint64-charitydonateaddr-string-charitydonateperc-uint64-uint64)
- [Function BuyItNow() Uint64](#function-buyitnow-uint64)
- [Function Bid() Uint64](#function-bid-uint64)
- [Function CloseListing() Uint64](#function-closelisting-uint64)
- [Function CancelListing() Uint64](#function-cancellisting-uint64)

## Function ClaimOwnership() Uint64
#### Parameters <!-- omit in toc -->
* None
#### Asset/DERO Inputs <!-- omit in toc -->
* 1 Asset input
#### Details <!-- omit in toc -->
* Used to claim ownership by supplying a value of 1 (0.00001 atomic units) asset. This is usually only ever necessary in the event of a private transfer through normal DERO txns. Listings (sale/auction) always transfer ownership at the end to the appropriate owner who wins the listing (or back to the owner in the event no bids/sales).

## Function Update(iconURL String, coverURL String, fileURL String, fileSignURL String, tags String) Uint64
#### Parameters <!-- omit in toc -->
* iconURL String
* coverURL String
* fileURL String
* fileSignURL String
* tags String
#### Asset/DERO Inputs <!-- omit in toc -->
* None
#### Details <!-- omit in toc -->
* Used to update stored values such as iconURL, coverURL, fileURL, fileSignURL, and tags. This can only be done by the creator by default, unless ownerCanUpdate is modified to be 1 rather than 0 (default). If ownerCanUpdate is 1, then the owner as well as the creator can run this function.

## Function Start(listType String, duration Uint64, startPrice Uint64, charityDonateAddr String, charityDonatePerc Uint64) Uint64
#### Parameters <!-- omit in toc -->
* listType String
* duration Uint64 (this is in hours)
* startPrice Uint64 (this is in atomic units. 500 for 0.00500 DERO, etc.)
* charityDonateAddr String
* charityDonatePerc Uint64 (0-100)
#### Asset/DERO Inputs <!-- omit in toc -->
* 1 Asset input
#### Details <!-- omit in toc -->
* Used to start a listing (sale/auction). Supplied parameters define the type of listing that it is. You can define a charity address and donation percentage to utilize on payout of the started listing if you choose.

## Function BuyItNow() Uint64
#### Parameters <!-- omit in toc -->
* None
#### Asset/DERO Inputs <!-- omit in toc -->
* DERO amount equal or greater to startPrice
#### Details <!-- omit in toc -->
* Used to buy an asset, assuming the supplied DERO amount is equal or greater than the startPrice. Sale listing types are single action, to which upon BuyItNow() within the appropriate time window, the buyer will receive the asset and owner receive the DERO.

## Function Bid() Uint64
#### Parameters <!-- omit in toc -->
* None
#### Asset/DERO Inputs <!-- omit in toc -->
* DERO amount equal or greater to currBidPrice
#### Details <!-- omit in toc -->
* Used to bid on an on-going auction listing type, assuming the supplied DERO amount is equal or greater to currBidPrice. Bids are refunded after being out-bid and the highest bidder at the end will win the auction.
  * **Please note:** currBidPrice is increased by either 10% or 0.1 DERO (depending which is lesser) after a bid is successfully placed.
* Auctions are closed out either by CloseListing() call by the owner or through a Bid() being placed after the end time is reached. If a bid is placed within the last 15 minutes of a bid window, the end window will be extended to 15 minutes.

## Function CloseListing() Uint64
#### Parameters <!-- omit in toc -->
* None
#### Asset/DERO Inputs <!-- omit in toc -->
* None
#### Details <!-- omit in toc -->
* Used to close out a finished listing (sale/auction). This will discard unknowingly (if using simulator) in the event a listing is still on-going. Only owners can run this function.

## Function CancelListing() Uint64
#### Parameters <!-- omit in toc -->
* None
#### Asset/DERO Inputs <!-- omit in toc -->
* None
#### Details <!-- omit in toc -->
* Used to cancel an on-going listing (sale/auction). This must be within the cancelBuffer variable timer, which is set to 300 seconds (or 5 minutes) after initiating a Start() call. Only owners can run this function.