

# Build with Dapper Sports Studio

## Introduction

<div>

Do you aspire to build the next big NFT collectible application, but
don't know where to begin? That was Dapper prior to CryptoKitties. Fast
forward to today and numerous successful sports products later - we want
to help you realize your goals of building alongside Dapper Sports
Studio!

</div>

<div>

Dapper Sports Studio is behind some of the biggest digital collectibles
products out there - including NBA TopShot, NFL All Day, UFC Strike and
Laliga Golazos.

</div>

<div>

This document highlights the overall architecture and best practices
when creating a Dapper Sports product. The audience is product managers
and developers who want to build on the excitement and magic of Dapper
Sports products themselves, reaching new fans. No technical experience
is required, although understanding concepts related to Digital
Collectibles (NFTs) and Blockchains (specifically FLOW) is beneficial.

</div>

## Core Concepts of a DSS Project

<div>

</div>

### Digital Collectibles

<div>

Digital Collectibles, also known as Non-Fungible Tokens (NFTs), are what
ultimately a user will own in their wallet. In the DSS world, these
collectibles are known as "Moments" since they capture a moment from a
sports game. Collectibles are derived from Editions. Editions, described
below, are like a template from which each Collectible is instantiated
(or "minted").

</div>

<div>

Each Digital Collectible has a unique serial number (represented by the
Edition number and a sequence of 1 - N as an example). This unique
serial number guarantees the uniqueness of the collectible and the only
one that will ever exist.

</div>

### Editions

<div>

Editions are templates from which Digital Collectibles are created (ie
minted). That is, you will have one Edition, but may have many Moments
created from that Edition. The number of moments created define the
rarity of that Edition. Editions are created from the following
parameters:

<div>

-   **Set:** A category for the Edition (e.g. Greatest Touchdowns).
-   **Series:** A period for the edition that has a defined beginning
    and end (e.g. 2023 Season, Series 1). Series have a defined end and
    are closed such that they can no longer be used in Editions once
    completed.
-   **Play:** A specific play from a sports game with associated
    metadata. Metadata will capture any important information about the
    play on-chain. Other data, such as the actual media clip, will be
    stored off-chain.

</div>

</div>

<div>

Each Edition will reference a SetId, SeriesId and PlayId to the
corresponding information.

</div>

### Packs

<div>

Several Digital Collectibles bundled together for sale. Packs have an
opening experience before revealing the collectibles in the Pack and
correspond to the idea of a "pack of cards" in the traditional trading
cards sense. Once the Pack is opened, the collectibles are stored in the
user's Wallet.

</div>

### Distributions

<div>

A Distribution is a collection of Packs. Similar to Editions, a
Distribution contains all of the required metadata to create the Packs.
Each Pack created from a Distribution can be purchased by users. There
can be different types of distributions:

</div>

-   Live Queue Drops that happen on a certain date for a certain period
    of time where users are assigned a random spot in the queue for
    purchasing their packs.
-   RSVP Drops where users commit their payment and receive a pack
    preventing the need to wait in a queue.
-   Air Drops refers to a promotional strategy of sending out Digital
    Collectibles for free to user Wallets.

## DSS Information Architecture

<div>

</div>

<div>

</div>

<div>

Now that we understand the basics of what makes up a Digital Collectible
in DSS, we can look at the overall architecture of a typical DSS App.

</div>

<div>

There are 4 basic parts to the architecture:

</div>

1.  **Front-End Client Application:** The FE can interact with back-end
    APIs that provide off-chain services, execute user approved
    transactions via the Dapper Wallet, or interact directly with the
    Flow Blockchain.
2.  **Back-End Services:** The back-end will provide a set of APIs that
    can be used by the Front-end to interact with both on-chain data,
    and off-chain data. This can include Databases, Media buckets,
    logging, caching, analytics, etc. Integrating both on-chain and
    off-chain data through the back-end is what creates the rich user
    experience of the app.
3.  **Flow Blockchain:** All required contracts, transaction and scripts
    are handled by the Flow Blockchain. This can be accessed directly
    from the front-end client or the back-end services. The Flow
    Blockchain is what provides the ownership of the collectibles and
    their uniqueness.
4.  **Dapper Wallet and Platform:** The Dapper Wallet and Platform
    provides payment, custody and identity services making it easy for
    users to purchase and sell their Digital Collectibles in a trusted,
    safe and secure way.

## Best Practices

### NFT Metadata Standard

<div>

One of the main issues of interoperability amongst different NFTs, is a
lack of standards in how the NFT metadata is represented. This makes it
difficult to add value to different NFT Collections if each of the
collections implement metadata differently.

</div>

<div>

To resolve this, Flow has created an NFT Metadata Standard which all NFT
Projects on flow are encouraged to use. The standard specifies that
important data should live on-chain, the metadata needs to be flexible
and can be extended by creating new metadata views.

</div>

<div>

For more information see:

</div>

-   Introducing NFT Metadata on Flow
-   NTF Metadata Contract

### Flow NFT Catalog

<div>

The Flow NFT Catalog is a tool designed to unlock the interoperability
of NFTs across the Flow Ecosystem. In order for an NFT Collection to
exist in the NFT Catalog, it must adhere to Flow NFT standards by
implementing the required interfaces. Once submitted, the NFT Collection
can be more easily discovered and integrated into other Flow
Applications.

</div>

### Example Contracts and Transactions

<div>

The NFL All Day Collections have been implemented using the latest
standards for NFTs on flow and are good examples to follow for your own
contracts. View the contracts here:

</div>

<div>

</div>

<div>

<div>

##### nfl-smart-contracts/README.md at main · dapperlabs/nfl-smart-contracts

Series encompass periods of time and will be named using strings like:
Summer 2021 or Series 3. More that one series can be open at any given
time, and in order for an Edition to be created, it must have a
SeriesID.

<div>

<div>

</div>

github.com

</div>

</div>

<div>

</div>

</div>

<div>

<div>

##### nfl-smart-contracts/AllDay.cdc at main · dapperlabs/nfl-smart-contracts

You can\'t perform that action at this time. You signed in with another
tab or window. You signed out in another tab or window. Reload to
refresh your session. Reload to refresh your session.

<div>

<div>

</div>

github.com

</div>

</div>

<div>

</div>

</div>

<div>

</div>

<div>

For examples of the transactions that are generated by the NFTCatalog
and use the recommended interfaces see the NFL All Day Pack Transactions
on NFTCatalog.

</div>

<div>

</div>

<div>

<div>

👉

</div>

<div>

**Now that you have an understanding of the core concepts of a Dapper
Sports Studio project, check out our** **Quickstart Guide** **to get
started building with Dapper.**

</div>

</div>

<div>

</div>

<div>

<div>

👋

</div>

<div>
