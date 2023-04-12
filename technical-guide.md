
# Learn the Basics - Technical Guides

<div>

<div>

</div>

<div>

The following guides have key information about interacting with Dapper
and FCL, and are a must-read prior to going to Mainnet.

</div>

</div>

<div>

</div>

### 1 -- Learn the FCL Basics

<div>

</div>

<div>

Ensure you check out the `Flow App Quickstart` tutorial to learn the
essentials of FCL (Flow Client Library)

</div>

<div>

<div>

##### Flow Developer Documentation

Last Updated: January 11th 2022 Note: This page will walk you through a
very bare bones project to get started building a web3 dapp using the
Flow Client Library (FCL). If you are looking for a clonable repo, Flow
community members have created quickstart templates for different
JavaScript frameworks (e.g.

<div>

<div>

</div>

developers.flow.com

</div>

</div>

<div>

</div>

</div>

### 2 -- Add FCL to your project

<div>

FCL is available on `npm` here:
https://www.npmjs.com/package/@onflow/fcl

</div>

<div>

<div>

💡

</div>

<div>

Dapper requires an `@onflow/fcl` version of at least
**`0.0.78-alpha.8`** or higher.

</div>

</div>

<div>

</div>

<div>

To install:

</div>

<div>

    // npm
    npm i -S @onflow/fcl
    // or if latest public release does not meet Dapper requirements
    npm i @onflow/fcl@0.0.78-alpha.9

    // yarn
    yarn add @onflow/fcl
    // or if latest public release does not meet Dapper requirements
    yarn add @onflow/fcl@0.0.78-alpha.9

</div>

<div>

</div>

<div>

</div>

## 3 -- Connect FCL to Dapper

<div>

Before a user can connect Dapper to an application, FCL must first be
configured to communicate with Dapper.

</div>

<div>

There are two ways to configure FCL for Dapper.

</div>

<div>

</div>

> **Configuration method 1: hard-code the wallet URI**

<div>

</div>

<div>

To connect a Flow-based application with Dapper, FCL must be explicitly
configured for Dapper.

</div>

<div>

    // lib/fcl/config.js
    import * as fcl from "@onflow/fcl";

    fcl.config({
      "discovery.wallet": process.env.DAPPER_WALLET_DISCOVERY,
      "discovery.wallet.method": process.env.DAPPER_WALLET_DISCOVERY_METHOD,
      "accessNode.api": process.env.FLOW_ACCESS_NODE,
      "app.detail.title": process.env.APP_NAME,
      "app.detail.icon": process.env.APP_ICON, //image url
    });

</div>

<div>

</div>

<div>

The environment variable values will be different between
`production / mainnet` and `staging / testnet` environments:

</div>

<div>

    // production/mainnet ENV vars
    DAPPER_WALLET_DISCOVERY =
      "https://accounts.meetdapper.com/fcl/authn-restricted";
    DAPPER_WALLET_DISCOVERY_METHOD = "POP/RPC";
    FLOW_ACCESS_NODE = "https://rest-mainnet.onflow.org";

</div>

<div>

    // staging/testnet ENV vars
    DAPPER_WALLET_DISCOVERY="https://staging.accounts.meetdapper.com/fcl/authn-restricted"
    DAPPER_WALLET_DISCOVERY_METHOD="POP/RPC"
    FLOW_ACCESS_NODE = "https://rest-testnet.onflow.org";

</div>

<div>

For more details on access nodes, please see the Flow documentation on
this subject:
https://developers.flow.com/nodes/access-api#flow-access-node-endpoints

</div>

<div>

The wallet discovery endpoints we configured here tells FCL to direct
user to the Dapper Wallet login/signup page.

</div>

<div>

The values for `app.detail.title` and `app.detail.icon` are used by
Dapper for cosmetic display purposes. At the time of this writing (Dec
13, 2021), only `app.detail.title` is used during the wallet connection
flow. If this value is set, the user will see
`Connecting to <app.detail.title>...`. If not set, the user will see
`Connecting...`

</div>

<div>

</div>

> **Configuration method 2: use FCL\'s wallet discovery page**

<div>

</div>

<div>

Using this method, FCL will show a pop-up window with a list of
supported wallets. Clicking on a specific wallet provider will further
configure FCL with details of that wallet.

</div>

<div>

</div>

<div>

</div>

<div>

To enable Dapper Wallet on the discovery page, configure FCL in the
following way:

</div>

<div>

    //for testnet
    fcl.config({
      "discovery.wallet": "https://fcl-discovery.onflow.org/testnet/authn",
      "discovery.authn.endpoint": "https://fcl-discovery.onflow.org/testnet/authn",
      "discovery.authn.include": [
        "0x82ec283f88a62e65", // Dapper Wallet
      ],
      //...rest of the config stays the same
    });

</div>

<div>

    //for mainnet
    fcl.config({
      "discovery.wallet": "https://fcl-discovery.onflow.org/authn",
      "discovery.authn.endpoint": "https://fcl-discovery.onflow.org/api/authn",
      "discovery.authn.include": [
        "0xead892083b3e2c6c", // Dapper Wallet
      ],
      //...rest of the config stays the same
    });

</div>

<div>

</div>

> **Connect a user\'s Dapper wallet to the application**

<div>

</div>

<div>

Once FCL is configured, a user\'s Dapper account can be connected by
calling `fcl.authenticate()`

</div>

<div>

    // app/components/login.jsx
    import { authenticate, unauthenticate, currentUser } from "@onflow/fcl" 
    import { useEffect } from "react"

    function Login() {
        
        const [user, setUser] = useState({ loggedIn: null });
        useEffect(() => currentUser().subscribe(setUser), [])

        return user.loggedIn 
        ? (
            <>
                <div>Welcome back {user.addr}!</div>
                <button onClick={unauthenticate}>Disconnect wallet</button>
            </>
        ) : (
            <button onClick={authenticate}>Connect wallet</button>
        )
    }

</div>

<div>

<div>

💡

</div>

<div>

When FCL is configured for Dapper, calling `authenticate()` will open a
new browser window to the specified Dapper URI. `authenticate()` must be
called in the same call stack as the click handler for the `button`
click, or the popup will be blocked by the browser (i.e. you cannot call
`authenticate()` asynchronous to the click handler).

</div>

</div>

<div>

See https://docs.onflow.org/fcl/flow-app-quickstart/#authentication for
a more in-depth example.

</div>

<div>

</div>

<div>

</div>

## 4 -- Use FCL to interact with a Dapper user

<div>

</div>

> **How to request a transaction signature from a Dapper user**

<div>

</div>

<div>

In order for a Cadence transaction to be executed on Flow, it must have
the appropriate cryptographic signatures. On Flow, there are 3 types of
signatures that must be provided: `Proposer`, `Payer`, and one ore more
`Authorizers` See this document for more details:

</div>

<div>

<div>

##### Signing a Transaction

Signing a transaction for Flow is a multi-step process that can involve
one or more accounts, each of which signs for a different purpose.
Proposer: the account that specifies a proposal key. Payer: the account
paying for the transaction fees. Authorizers: zero or more accounts
authorizing the transaction to mutate their state.

<div>

<div>

</div>

developers.flow.com

</div>

</div>

<div>

</div>

</div>

<div>

See the FCL documentation for guidance on how to request transaction
signatures from a wallet, including Dapper:

</div>

<div>

<div>

##### Flow Developer Documentation

FCL has a mechanism that lets you configure various aspects of FCL. When
you move from one instance of the Flow Blockchain to another (Local
Emulator to Testnet to Mainnet) the only thing you should need to change
for your FCL implementation is your configuration. Values only need to
be set once.

<div>

<div>

</div>

developers.flow.com

</div>

</div>

<div>

</div>

</div>

<div>

Using FCL, follow the example of this code snippet to submit Dapper
transaction through FCL.

</div>

<div>

    // This FCL code snippet showcases FCL code interaction with the Dapper Wallet
    const dapperAuthz = fcl.authz;

        const tx = await fcl.send([
          fcl.transaction(buyTx),
          fcl.payer(fcl.authz),
          fcl.proposer(fcl.authz),
          fcl.authorizations([dapperAuthz, fcl.authz]),
          fcl.args([
            fcl.arg(Number(30227649), t.UInt64),
            fcl.arg('0x8fb4a6a11757b80d', t.Address),
            fcl.arg(Number(100).toFixed(8), t.UFix64),
          ]),
          fcl.limit(1000),
        ]).then(tx => {
          return fcl.decode(tx);
        });

</div>

> ⚠️ WARNING: **Popup blocking behavior in Safari and Firefox**

<div>

</div>

<div>

<div>

💡

</div>

<div>

This warning pertains specifically to Safari and Firefox browsers.
Chrome does not currently exhibit this behavior.

</div>

</div>

<div>

</div>

<div>

FCL will attempt to open a popup window to Dapper to request transaction
signatures. Default browser behavior is to block the popup if there is
any asynchronous action between when the user clicks the button to
request the signature and when the call to `fcl.send` or `fcl.mutate` is
made.

</div>

<div>

</div>

<div>

For example, this code will result in Safari or Firefox blocking the
Dapper popup:

</div>

<div>

    // This will result in the popup being blocked by Safari and Firefox
    async function handleBuyClick() {
        const reserveredNftId = await api.reserveNft();
        await fcl.mutate({ ... })
    }

</div>

<div>

Please thoroughly test with Safari/Firefox any code paths that request
signatures from Dapper to ensure that the popup is not blocked.

</div>

<div>

</div>

> **How to query Flow for account information on a Dapper user**

<div>

Information about a user\'s Flow account can be queried using Cadence
scripts. Scripts do not require authorization from the user and can be
accessed from public information made available on their account.

</div>

<div>

See the FCL documentation on how to send scripts to the chain:

</div>

<div>

<div>

##### Flow Documentation

Version: 0.76.0 Last Updated: August 13, 2021 FCL has a mechanism that
lets you configure various aspects of FCL. When you move from one
instance of the Flow Blockchain to another (Local Emulator to Testnet to
Mainnet) the only thing you should need to change for your FCL
implementation is your configuration.

<div>

<div>

</div>

docs.onflow.org

</div>

</div>

</div>

<div>

</div>

## 5 -- Dapper signing transactions

<div>

</div>

> **Dapper signer roles**

<div>

</div>

<div>

For Dapper users, the transaction signatures provided will be a mix of
the user\'s keys and Dapper admin keys.

</div>

<div>

</div>

> **User-provided signatures**

<div>

</div>

<div>

A Dapper user will provide a signature for the`Proposer` and possibly an
`Authorizer`. Users **do not** provide a `Payer` signature.

</div>

<div>

An example of a transaction where the user provides both `Proposer` and
`Authorizer` signatures is the \'**initialization**\' transaction here.

</div>

<div>

An example of a transaction where the user only provides the `Proposer`
signature here.

</div>

<div>

</div>

> **Dapper-provided signatures**

<div>

</div>

<div>

A Dapper admin account will provide a signature for the `Payer` and
possibly an `Authorizer`.

</div>

<div>

By providing the `Payer` signature, Dapper pays for transaction fees on
behalf of its users.

</div>

<div>

Dapper provides the `Authorizer` signature when it needs access to
private resources in the Dapper Wallet admin account, such as the case
when a Dapper user purchases an NFT from a marketplace. See the \'**buy
listing**\' transaction here.

</div>

<div>

</div>

> **Supported transactions on Dapper**

<div>

</div>

<div>

Because Dapper is a custodial wallet, **transactions signatures will
only be provided for specific, known transaction scripts.** This is to
prevent transactions that are malicious or against our policies from
being executed.

</div>

<div>

</div>

<div>

As part of the engagement process with marketplaces wishing to integrate
with Dapper, the Dapper team will review all marketplace transaction
templates to make sure they are compliant with our policies and, in the
case of purchase-type transactions, will work with our payment system.

</div>

<div>

</div>

<div>

Once the transactions are reviewed and approved by the Dapper team, they
will be added to an \'**allow-list**\' on the Dapper platform, which
will allow Dapper users to sign them.

</div>

<div>

</div>

## 6 -- Initialize a user\'s Flow account for a marketplace

<div>

A user\'s Flow account, typically, needs the specific marketplace
`resources` and `capabilities` to be created in their account in order
to be able to execute the Cadence transactions that underpin the
marketplace.

</div>

<div>

For a typical marketplace, two types of `resources` need to be created:

</div>

-   specific NFT resources to be able to store, deposit, and withdraw
    NFTs
-   an NFT Storefront resource to be able to list NFTs for sale

<div>

</div>

<div>

<div>

💡

</div>

<div>

The `NFTStorefront` resource only needs to be deployed once per Flow
account, so it is unlikely that the marketplace will need to initialize
this resource for the user

</div>

</div>

<div>

`Capabilities` are created on top of these resources that expose
specific functionality to allow the marketplace to interact with the
user\'s resources in a secure manner.

</div>

<div>

</div>

> **Required marketplace resources**

<div>

</div>

<div>

A marketplace is responsible for writing the Cadence transactions that
will check for, and create if missing, the specific NFT resources and
capabilities that are required to interact with the marketplace
contracts and transactions.

</div>

<div>

</div>

<div>

Whether a marketplace only allows one type of NFT (NBA Top Shot) or
allows many different NFT types (VIV3), the process of checking for the
specific NFT resource and creating it if it\'s missing, is largely the
same.

</div>

<div>

</div>

> **NFT Storefront resource**

<div>

</div>

<div>

The `NFTStorefront` resource can handle any type of NFT. **During the
Dapper account creation process**, an `NFTStorefront` resource will be
created in the user\'s Flow account, so the marketplace should not have
to create this for the user. It is recommended that marketplace
transactions still check for the existence of this resource.

</div>

<div>

</div>

<div>

The `NFTStorefront` contract, scripts, and transactions are open source:

</div>

<div>

<div>

##### GitHub - onflow/nft-storefront: A general-purpose Cadence contract for trading NFTs on Flow

The NFT storefront is a general-purpose Cadence contract for trading
NFTs on Flow. NFTStorefront uses modern Cadence run-time type facilities
to implement a marketplace that can take any currency in order to vend
any token in a safe and secure way.

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

The Cadence transaction to add this resource and expose capabilities can
be found here.

</div>

> **NFT resources**

<div>

</div>

<div>

There are two approaches a marketplace may take when attempting to add
NFT resources to a user\'s Flow account:

</div>

-   a one-time \'**initialization**\' transaction
-   as part of a transaction that requires the NFT resource to exist,
    such as a \'**Buy NFT**\' transaction

<div>

It is preferable to initialize the user\'s account as a one-time
\'**initialization**\' transaction to ensure can interact with all
marketplace transactions (such as gifting, for example).

</div>

<div>

</div>

> **User initialization transaction**

<div>

</div>

<div>

After a user connects their Dapper wallet to the marketplace for the
first time, the marketplace can ask the user to sign a transaction that
adds the correct resources and capabilities to their Flow account. By
initializing the user\'s Flow account as part of the first-time
connection experience, the user is able to receive the marketplace NFTs
(again, such as receiving a gift).

</div>

<div>

</div>

<div>

An example initialization
transaction(https://github.com/dapperlabs/dapper-supported-transactions/blob/master/initialize-account/initialize-account.cdc)

</div>

<div>

    import NFTStorefront from ${NFTStorefrontContractAddress}
    import DapperUtilityCoin from ${DapperUtilityCoinContractAddress}
    import FungibleToken from ${FungibleTokenContractAddress}
    import ${NFTContractName} from ${NFTContractAddress}

    // This transcation initializes an account with a collection that allows it to hold NFTs from a specific contract. It will
    // do nothing if the account is already initialized.
    transaction {
        prepare(collector: AuthAccount) {
            if collector.borrow<&${NFTContractName}.Collection>(from: ${NFTContractName}.CollectionStoragePath) == nil {
                let collection <- ${NFTContractName}.createEmptyCollection() as! @${NFTContractName}.Collection
                collector.save(<-collection, to: ${NFTContractName}.CollectionStoragePath)
                collector.link<&{${NFTContractName}.Collection{NonFungibleToken.CollectionPublic, MetadataViews.ResolverCollection}>(
                    ${NFTContractName}.CollectionPublicPath,
                    target: ${NFTContractName}.CollectionStoragePath,
                )
            }
        }
    }

</div>

<div>

</div>

<div>

If a marketplace deals with multiple type of NFTs, the initialization
transaction would add each NFT resource and capability one by one:

</div>

<div>

    import NonFungibleToken from 0xNFTADDRESS
    import FooNFT from 0xFOONFTCONTRACTADDRESS
    import BarNFT from 0xBARNFTCONTRACTADDRESS

    transaction {

        prepare(acct: AuthAccount) {
                    // Check if the FooNFT resource already exists
            if acct.borrow<&FooNFT.Collection>(from: /storage/FooNFTCollection) == nil {
                let collection <- FooNFT.createEmptyCollection()
                acct.save(<-collection, to: /storage/FooNFTCollection)
                acct.link<&{NonFungibleToken.CollectionPublic}>(
                    /public/FooNFTCollection,
                    target: /storage/FooNFTCollection
                )
                 }

                 // Check if the BarNFT resource already exists
          if acct.borrow<&BarNFT.Collection>(from: /storage/BarNFTCollection) == nil {
            let collection <- BarNFT.createEmptyCollection()
            acct.save(<-collection, to: /storage/BarNFTCollection)
            acct.link<&{NonFungibleToken.CollectionPublic}>(
                /public/BarNFTCollection,
                target: /storage/BarNFTCollection
            )
             }
        }
    }

</div>

<div>

</div>

> **As part of a \'buy\' transaction**

<div>

</div>

<div>

A potential downside of the one-time \'**initialization**\' transaction
described above is the extra step a user must take before being able to
fully interact with the marketplace transactions. Instead, the NFT
resources can be added as part of the \'**buy**\' transaction, if they
are missing from the user\'s account.

</div>

<div>

A slightly modified example from the `purchase-nft-direct`
transaction(https://github.com/dapperlabs/dapper-supported-transactions/blob/master/purchase-nft-direct/purchase-nft-direct.cdc):

</div>

<div>

</div>

<div>

    import FungibleToken from ${FungibleTokenContractAddress}
    import NonFungibleToken from ${NonFungibleTokenContractAddress}
    import NFTStorefront from ${NFTStorefrontContractAddress}
    import DapperUtilityCoin from ${DapperUtilityCoinContractAddress}
    import ${NFTContractName} from ${NFTContractAddress}

    // This transaction purchases an NFT from a dapp directly (i.e. **not** on a peer-to-peer marketplace).
    // FIRST ARGUMENT OF THIS TRANSACTION MUST BE YOUR MERCHANT ACCOUNT ADDRESS PROVIDED BY DAPPER LABS
    transaction(merchantAccountAddress: Address, storefrontAddress: Address, listingResourceID: UInt64, expectedPrice: UFix64) {
        let paymentVault: @FungibleToken.Vault
        let buyerNFTCollection: &AnyResource{NonFungibleToken.CollectionPublic}
        let storefront: &NFTStorefront.Storefront{NFTStorefront.StorefrontPublic}
        let listing: &NFTStorefront.Listing{NFTStorefront.ListingPublic}
        let balanceBeforeTransfer: UFix64
        let mainDUCVault: &DapperUtilityCoin.Vault
        let dappAddress: Address
        let salePrice: UFix64
        
        // "dapp" as authorizer is OPTIONAL
        // If "dapp" is also an authorizer and it MUST be the first authorizer
        prepare(dapp: AuthAccount, dapper: AuthAccount, buyer: AuthAccount) {
            self.dappAddress = dapp.address

            // Initialize the collection if the buyer does not already have one
            if buyer.borrow<&${NFTContractName}.Collection>(from: ${NFTContractName}.CollectionStoragePath) == nil {
                buyer.save(<-${NFTContractName}.createEmptyCollection(), to: ${NFTContractName}.CollectionStoragePath
                buyer.link<&{${NFTContractName}.Collection{NonFungibleToken.CollectionPublic, MetadataViews.ResolverCollection}>(
                    ${NFTContractName}.CollectionPublicPath,
                    target: ${NFTContractName}.CollectionStoragePath
                )
                 ?? panic("Could not link collection Pub Path");
            }

            self.storefront = dapp
                .getCapability<&NFTStorefront.Storefront{NFTStorefront.StorefrontPublic}>(NFTStorefront.StorefrontPublicPath)
                .borrow()
                ?? panic("Could not borrow a reference to the storefront")
            self.listing = self.storefront.borrowListing(listingResourceID: listingResourceID)
                ?? panic("No Listing with that ID in Storefront")

            self.salePrice = self.listing.getDetails().salePrice

            self.mainDUCVault = dapper.borrow<&DapperUtilityCoin.Vault>(from: /storage/dapperUtilityCoinVault)
                        ?? panic("Could not borrow reference to Dapper Utility Coin vault")
            self.balanceBeforeTransfer = self.mainDUCVault.balance
            self.paymentVault <- self.mainDUCVault.withdraw(amount: self.salePrice)

            self.buyerNFTCollection = buyer
                .getCapability<&{NonFungibleToken.CollectionPublic}>(PackNFT.CollectionPublicPath)
                .borrow()
                ?? panic("Cannot borrow NFT collection receiver from account")
        }

        pre {
            self.salePrice == expectedPrice: "unexpected price"
            self.dappAddress == ${NFTContractAddress} && self.dappAddress == storefrontAddress: "Requires valid authorizing signature"
        }

        execute {
            let item <- self.listing.purchase(
                payment: <-self.paymentVault
            )

            self.buyerNFTCollection.deposit(token: <-item)
        }

        post {
            self.mainDUCVault.balance == self.balanceBeforeTransfer: "DUC leakage"
        }
    }

</div>

<div>

</div>

> **Detecting if a user\'s Flow account is initialized**

<div>

</div>

<div>

A marketplace can determine if a user has already initialized their Flow
account for the marketplace by querying their on-chain account for the
required resources and capabilities.

</div>

<div>

For example, to check if a specific NFT resource and capability used by
the marketplace has been added to a user\'s Flow account, a marketplace
could run the following script, which returns a `boolean` indicating
whether or not the account is initialized.

</div>

<div>

    import NonFungibleToken from OxNFTADDRESS
    import ExampleNFT from 0xEXAMPLENFTADDRESS

    pub fun main(userAddress: Address): Bool {
      return getAccount(userAddress)
        .getCapability<&ExampleNFT.Collection{NonFungibleToken.Receiver}>(ExampleNFT.CollectionPublicPath)
        .check()
    }

</div>

<div>

</div>

## 7 -- Initialize the Storefront account

<div>

</div>

> **Setting up the Storefront resource**

<div>

</div>

<div>

The Flow account that will be selling direct-to-user NFTs will need to
create and store a `NFTStorefront.Storefront` resource, by running this
transaction:

</div>

<div>

<div>

##### nft-storefront/setup_account.cdc at main · onflow/nft-storefront

A general-purpose Cadence contract for trading NFTs on Flow -
nft-storefront/setup_account.cdc at main · onflow/nft-storefront

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

> **Receiving on-chain payments from Dapper users transaction**

<div>

</div>

<div>

For marketplaces that will sell from a central Storefront account (e.g
direct-to-user, usually used for \'drops\' or other non-p2p sales), a
special type of fungible token resource will need to be created on the
Storefront account in order to integrate with Dapper\'s payment system.

</div>

<div>

</div>

<div>

To receive on-chain payments from a Dapper user, a specific fungible
token resources needs to be added to the Storefront account.

</div>

<div>

</div>

<div>

A Storefront account is defined here as an account controlled by the
marketplace operator, used for direct-to-consumer type sales such as NFT
\'drops\'.

</div>

<div>

</div>

<div>

When a Dapper user purchases an NFT listing from the Storefront, two
distinct payment events happen: one off-chain, and one on-chain:

</div>

-   **Off-chain:** a Dapper user can purchase an NFT using Dapper
    credits, credit cards, ACH payment, and cryptocurrencies from other
    chains. Dapper collects the user\'s payment on behalf of the
    Storefront.
-   **On-chain**: after collecting the user\'s off-chain payment, Dapper
    purchases the NFT on behalf of a user, using an on-chain fungible
    token called `DapperUtilityCoin` (DUC). As the name implies, the
    Dapper Utility Coin is used strictly for utility and has no
    intrinsic value. Please note, DUC is intended only to be used within
    Dapper products.

<div>

</div>

<div>

The DUC contract is deployed here:

</div>

<div>

<div>

##### flowscan.org

<div>

flowscan.org

</div>

</div>

</div>

<div>

</div>

<div>

To receive the payment on-chain in DUC, the Storefront account must
create a special resource called a `Forwarder`. The `Forwarder` ensures
that the Storefront is properly credited for purchases made by Dapper
users.

</div>

<div>

The `TokenForwarding` contract, which defines the `Forwarder` resource,
is deployed here:

</div>

<div>

<div>

##### flowscan.org

<div>

flowscan.org

</div>

</div>

</div>

<div>

</div>

<div>

An example of the transaction to run on the Storefront account:

</div>

<div>

    //add fut forwarder receiver
    import FungibleToken from 0x9a0766d93b6608b7
    import TokenForwarding from 0x51ea0e37c27a1f1a
    import FlowUtilityToken from 0x82ec283f88a62e65

    /** 
      To receive the payment on-chain in FUT, the Storefront account must create a special resource called a Forwarder. 
      The Forwarder ensures that the Storefront is properly credited for purchases made by Dapper users. 
    **/

    transaction(dapperAccountAddress: Address) {

        prepare(acct: AuthAccount) {
            // Get a Receiver reference for the Dapper account that will be the recipient of the forwarded FUT
            let dapper = getAccount(dapperAccountAddress)
            let dapperFUTReceiver = dapper.getCapability(/public/flowUtilityTokenReceiver)!

            // Create a new Forwarder resource for FUT and store it in the new account''s storage
            let futForwarder <- TokenForwarding.createNewForwarder(recipient: dapperFUTReceiver)
            acct.save(<-futForwarder, to: /storage/flowUtilityTokenReceiver)

            // Publish a Receiver capability for the new account, which is linked to the FUT Forwarder
            acct.link<&{FungibleToken.Receiver}>(/public/flowUtilityTokenReceiver, target: /storage/flowUtilityTokenReceiver)
        }
    }

</div>

<div>

    //add duc forwarder receiver
    import FungibleToken from 0x9a0766d93b6608b7
    import TokenForwarding from 0x51ea0e37c27a1f1a
    import DapperUtilityCoin from 0x82ec283f88a62e65

    /** 
      To receive the payment on-chain in DUC, the Storefront account must create a special resource called a Forwarder. 
      The Forwarder ensures that the Storefront is properly credited for purchases made by Dapper users. 
    **/

    transaction(dapperAccountAddress: Address) {

        prepare(acct: AuthAccount) {
            // Get a Receiver reference for the Dapper account that will be the recipient of the forwarded DUC
            let dapper = getAccount(dapperAccountAddress)
            let dapperDUCReceiver = dapper.getCapability(/public/dapperUtilityCoinReceiver)!

            // Create a new Forwarder resource for DUC and store it in the new account''s storage
            let ducForwarder <- TokenForwarding.createNewForwarder(recipient: dapperDUCReceiver)
            acct.save(<-ducForwarder, to: /storage/dapperUtilityCoinReceiver)

            // Publish a Receiver capability for the new account, which is linked to the DUC Forwarder
            acct.link<&{FungibleToken.Receiver}>(/public/dapperUtilityCoinReceiver, target: /storage/dapperUtilityCoinReceiver)
        }
    }

</div>

<div>

<div>

💡

</div>

<div>

The `TokenForwarding.Forwarder` resource forwards, or routes, the DUC
payment back to the Dapper admin account, such that the Storefront
account never actually holds any DUC. Instead, on-chain events are
emitted by the `TokenForwarding` contract whenever a payment in DUC is
received. These events are consumed by the Dapper platform to credit the
storefront operator with Dapper credits. These credits can later be
withdrawn at the storefront operator\'s request.

</div>

</div>

<div>

</div>

<div>

</div>

## 8 -- List an NFT for sale

<div>

</div>

> **Listing an NFT for sale for non-Dapper users**

<div>

</div>

<div>

An NFT can be listed for sale by creating a `Listing` resource in the
seller\'s Flow account. See the following transaction that lists an
`ExampleNFT.NFT` for sale:

</div>

<div>

<div>

##### nft-storefront/sell_item.cdc at main · onflow/nft-storefront

This file contains bidirectional Unicode text that may be interpreted or
compiled differently than what appears below. To review, open the file
in an editor that reveals hidden Unicode characters. Learn more about
bidirectional Unicode characters You can\'t perform that action at this
time. You signed in with another tab or window.

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

> **Listing an NFT for sale for Dapper users**

<div>

</div>

<div>

For a Dapper user to be able to purchase an NFT, the transaction that
creates the `Listing` transaction needs to be different than the above
example so that:

</div>

-   Dapper users can pay for an NFT using off-chain payment methods,
    such as Dapper Balance, credit cards, ACH, wires and/or
    cryptocurrencies on other chains

<div>

</div>

<div>

For example:

</div>

<div>

    import FungibleToken from ${FungibleTokenContractAddress}
    import NonFungibleToken from ${NonFungibleTokenContractAddress}
    import DapperUtilityCoin from ${DapperUtilityCoinContractAddress}
    import NFTStorefront from ${NFTStorefrontContractAddress}
    import ${NFTContractName} from ${NFTContractAddress}

    // This transcation can be used to place and NFT for sale on a marketplace such that a specified percentage of the proceeds of the sale
    // go to the dapp as a royalty.
    transaction(saleItemID: UInt64, saleItemPrice: UFix64, royaltyPercent: UFix64) {
        let sellerPaymentReceiver: Capability<&{FungibleToken.Receiver}>
        let nftProvider: Capability<&${NFTContractName}.Collection{NonFungibleToken.Provider, NonFungibleToken.CollectionPublic}>
        let storefront: &NFTStorefront.Storefront
        let dappAddress: Address

        // It's important that the dapp account authorize this transaction so the dapp has the ability
        // to validate and approve the royalty included in the sale.
        // "dapp" as authorizer is not needed if you hardcode royalty/royaltyRecipient in the transaction
        prepare(dapp: AuthAccount, seller: AuthAccount) {
            self.dappAddress = dapp.address

            // If the account doesn't already have a storefront, create one and add it to the account
            if seller.borrow<&NFTStorefront.Storefront>(from: NFTStorefront.StorefrontStoragePath) == nil {
                let newstorefront <- NFTStorefront.createStorefront() as! @NFTStorefront.Storefront
                seller.save(<-newstorefront, to: NFTStorefront.StorefrontStoragePath)
                seller.link<&NFTStorefront.Storefront{NFTStorefront.StorefrontPublic}>(
                    NFTStorefront.StorefrontPublicPath,
                    target: NFTStorefront.StorefrontStoragePath
                )
            }

            // Get a reference to the receiver that will receive the fungible tokens if the sale executes.
            // Note that the sales receiver aka MerchantAddress should be an account owned by Dapper or an end-user Dapper Wallet account address.
            self.sellerPaymentReceiver = getAccount(${MerchantAddress}).getCapability<&{FungibleToken.Receiver}>(/public/dapperUtilityCoinReceiver)
            assert(self.sellerPaymentReceiver.borrow() != nil, message: "Missing or mis-typed DapperUtilityCoin receiver")

            // If the user does not have their collection linked to their account, link it.
            let nftProviderPrivatePath = /private/${NFTContractName}CollectionProviderForNFTStorefront
            let hasLinkedCollection = seller.
                getCapability<&${NFTContractName}.Collection{NonFungibleToken.Provider, NonFungibleToken.CollectionPublic}>(
                    nftProviderPrivatePath
                )!.check()
            if !hasLinkedCollection {
                seller.link<&${NFTContractName}.Collection{NonFungibleToken.Provider, NonFungibleToken.CollectionPublic}>(
                    nftProviderPrivatePath,
                    target: ${NFTContractName}.CollectionStoragePath
                )
            }

            // Get a capability to access the user's NFT collection.
            self.nftProvider = seller.
                getCapability<&${NFTContractName}.Collection{NonFungibleToken.Provider, NonFungibleToken.CollectionPublic}>(
                    nftProviderPrivatePath
                )!
            assert(self.nftProvider.borrow() != nil, message: "Missing or mis-typed collection provider")

            // Get a reference to the user's NFT storefront
            self.storefront = seller.borrow<&NFTStorefront.Storefront>(from: NFTStorefront.StorefrontStoragePath)
                ?? panic("Missing or mis-typed NFTStorefront Storefront")
        }

        // Make sure dapp is actually the dapp and not some random account
        pre {
            self.dappAddress == ${NFTContractAddress}: "Requires valid authorizing signature"
        }

        execute {
            // Calculate the amout the seller should receive if the sale executes, and the amount
            // that should be sent to the dapp as a royalty.
            let amountSeller = saleItemPrice * (1.0 - royaltyPercent)
            let amountRoyalty = saleItemPrice - amountSeller

            // Get the royalty recipient's public account object
            // Note that the royalty receiver should be an account owned by Dapper (aka MerchantAddress) or an end-user Dapper Wallet account address.
            let royaltyRecipient = getAccount(${RoyaltyReceiverAddress})

            // Get a reference to the royalty recipient's Receiver
            let royaltyReceiverRef = royaltyRecipient.getCapability<&{FungibleToken.Receiver}>(/public/dapperUtilityCoinReceiver)
            assert(royaltyReceiverRef.borrow() != nil, message: "Missing or mis-typed DapperUtilityCoin royalty receiver")

            let saleCutSeller = NFTStorefront.SaleCut(
                receiver: self.sellerPaymentReceiver,
                amount: amountSeller
            )

            let saleCutRoyalty = NFTStorefront.SaleCut(
                receiver: royaltyReceiverRef,
                amount: amountRoyalty
            )

            self.storefront.createListing(
                nftProviderCapability: self.nftProvider,
                nftType: Type<@${NFTContractName}.NFT>(),
                nftID: saleItemID,
                salePaymentVaultType: Type<@DapperUtilityCoin.Vault>(),
                saleCuts: [saleCutSeller, saleCutRoyalty]
            )
        }
    }

</div>

<div>

</div>

## 9 -- Buy an NFT from a marketplace

<div>

</div>

<div>

A Dapper user can purchase an NFT from a marketplace if the
`NFTStorefront.Listing` resource representing the sale was created to
accept the Dapper Utility Coin (DUC) as payment. For more info, please
see here.

</div>

<div>

</div>

<div>

When purchasing an NFT from a compatible marketplace, a Dapper user
purchases the NFT using Dapper checkout and an off-chain payment method
such as credit card, Dapper credits, ACH, or cryptocurrencies on other
chains.

</div>

<div>

</div>

<div>

The payment is collected by Dapper on behalf of the marketplace (seller)
and Dapper purchases the NFT on behalf of the user (buyer). On-chain,
Dapper purchases the `Listing` with DUC and deposits the NFT in the
buyer\'s NFT collection. The seller receives the payment in the form of
Dapper credits.

</div>

<div>

</div>

> **High-level \'buy NFT\' flow**

<div>

</div>

<div>

</div>

<div>

</div>

> **An example buy transaction**

<div>

</div>

<div>

    import FungibleToken from ${FungibleTokenContractAddress}
    import NonFungibleToken from ${NonFungibleTokenContractAddress}
    import DapperUtilityCoin from ${DapperUtilityCoinContractAddress}
    import NFTStorefront from ${NFTStorefrontContractAddress}
    import ${NFTContractName} from ${NFTContractAddress}

    // This transaction purchases an NFT on a peer-to-peer marketplace (i.e. **not** directly from a dapp). This transaction
    // will also initialize the buyer's NFT collection on their account if it has not already been initialized.
    // FIRST ARGUMENT OF A P2P PURCHASE TRANSACTION SHOULD ALWAYS BE THE SELLER'S ADDRESS
    transaction(storefrontAddress: Address, listingResourceID: UInt64,  expectedPrice: UFix64) {
        let paymentVault: @FungibleToken.Vault
        let nftCollection: &${NFTContractName}.Collection{NonFungibleToken.Receiver}
        let storefront: &NFTStorefront.Storefront{NFTStorefront.StorefrontPublic}
        let listing: &NFTStorefront.Listing{NFTStorefront.ListingPublic}
        let salePrice: UFix64
        let balanceBeforeTransfer: UFix64
        let mainDapperUtilityCoinVault: &DapperUtilityCoin.Vault

        prepare(dapper: AuthAccount, buyer: AuthAccount) {
            // Initialize the buyer's collection if they do not already have one
            if buyer.borrow<&${NFTContractName}.Collection>(from: ${NFTContractName}.CollectionStoragePath) == nil {
                let collection <- ${NFTContractName}.createEmptyCollection() as! @${NFTContractName}.Collection
                buyer.save(<-collection, to: ${NFTContractName}.CollectionStoragePath)
                
                buyer.link<&{${NFTContractName}.Collection{NonFungibleToken.CollectionPublic, MetadataViews.ResolverCollection}>(
                    ${NFTContractName}.CollectionPublicPath,
                    target: ${NFTContractName}.CollectionStoragePath
                )
                 ?? panic("Could not link collection Pub Path");
            }

            // Get the storefront reference from the seller
            self.storefront = getAccount(storefrontAddress)
                .getCapability<&NFTStorefront.Storefront{NFTStorefront.StorefrontPublic}>(
                    NFTStorefront.StorefrontPublicPath
                )!
                .borrow()
                ?? panic("Could not borrow Storefront from provided address")

            // Get the listing by ID from the storefront
            self.listing = self.storefront.borrowListing(listingResourceID: listingResourceID)
                ?? panic("No Offer with that ID in Storefront")
            self.salePrice = self.listing.getDetails().salePrice

            // Get a DUC vault from Dapper's account
            self.mainDapperUtilityCoinVault = dapper.borrow<&DapperUtilityCoin.Vault>(from: /storage/dapperUtilityCoinVault)
                ?? panic("Cannot borrow DapperUtilityCoin vault from account storage")
            self.balanceBeforeTransfer = self.mainDapperUtilityCoinVault.balance
            self.paymentVault <- self.mainDapperUtilityCoinVault.withdraw(amount: self.salePrice)

            // Get the collection from the buyer so the NFT can be deposited into it
            self.nftCollection = buyer.borrow<&${NFTContractName}.Collection{NonFungibleToken.Receiver}>(
                from: ${NFTContractName}.CollectionStoragePath
            ) ?? panic("Cannot borrow NFT collection receiver from account")
        }

        // Check that the price is right
        pre {
            self.salePrice == expectedPrice: "unexpected price"
        }

        execute {
            let item <- self.listing.purchase(
                payment: <-self.paymentVault
            )

            self.nftCollection.deposit(token: <-item)

            // Remove listing-related information from the storefront since the listing has been purchased.
            self.storefront.cleanup(listingResourceID: listingResourceID)
        }

        // Check that all dapperUtilityCoin was routed back to Dapper
        post {
            self.mainDapperUtilityCoinVault.balance == self.balanceBeforeTransfer: "DapperUtilityCoin leakage"
        }
    }

</div>

<div>

</div>

## 10 -- Transaction metadata

<div>

Transactions can have metadata associated with them in order to display
a user-facing description of an underlying Cadence transaction script.
Dapper (will) support two types of metadata: **off-chain** and
**on-chain**.

</div>

<div>

</div>

> **Off-chain metadata**

<div>

</div>

<div>

Off-chain metadata is generic and does not take into account the
specifics of a transaction, such as the arguments to it.

</div>

<div>

For example, many applications require that a user's Flow account be
properly initialized so that it can hold the application's specific
NFTs. A high-level description of such a transaction might read:
**"Prepare your Flow account for our awesome NFTs."** This would be
displayed more prominently than the Cadence transaction script that it
describes since most users do not understand Cadence.

</div>

<div>

</div>

> **On-chain metadata**

<div>

</div>

<div>

On the other hand, on-chain metadata can be more specific than off-chain
metadata because Dapper can use transaction arguments to query Flow for
details about a transaction.

</div>

<div>

For example, a '**buy NFT'** transaction will specify transaction
arguments such as **NFT_ID** and price. Dapper can then run a Cadence
script, using those argument values, to query for metadata about that
particular NFT (if it exists on-chain). A hypothetical NFT might define
metadata such as **'serial number'**, **'edition name'**, **'series
name'**, etc. which can be queried by **NFT_ID**. Dapper Checkout then
uses this mechanism to populate the Checkout screen with the **name,
serial number, price,** **and image** of the NFT.

</div>

<div>

Another example would be listing an NFT for sale. The arguments to this
transaction would be similar to the **'buy NFT'** transaction: the
**NFT_ID** and the price. Dapper can run an on-chain query using these
arguments to display a screen to the user listing the NFT for sale with
the metadata reading **"Do you want to list Furry Octopus #14/100 for
\$99 USD?"** alongside an image of the Furry Octopus. For more details
on how Dapper manages assets, please see our FAQ entry titled 'How do I
provide images/assets to Dapper?'

</div>

<div>

</div>

## 11 -- On-chain metadata

<div>

On-chain metadata is needed for Dapper checkout to ensure that the asset
description and price displayed to the user matches the asset
description and price on-chain.

</div>

<div>

<div>

##### GitHub - onflow/flow-nft: The non-fungible token standard on the Flow blockchain

This standard defines the minimum functionality required to implement a
safe, secure, and easy-to-use non-fungible token contract on the Flow
blockchain. Cadence is the resource-oriented programming language for
developing smart contracts on Flow. Before reading this standard, we
recommend completing the Cadence tutorials to build a basic
understanding of the programming language.

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

For a more bespoke implementation, an NFT contract can define a metadata
structure particular to that NFT. For example, the `Genies`contract
defines metadata using a hierarchy of on-chain `structs` *singletons*
that are referenced by the NFTs. This allows the metadata to be stored
on-chain at minimal cost, as opposed to duplicating and storing the
metadata *within* the NFT resource itself (storing data on-chain incurs
storage costs).

</div>

<div>

</div>

<div>

In the `Genies` contract, the only data stored on the `NFT` itself is
the `serialNumber`, the `mintingDate`, and a reference to the `Edition`
from which it was minted. The rest of the metadata for an NFT can be
retrieved by querying these other structs, using the `NFT.id` as the
starting point.

</div>

<div>

</div>

<div>

<div>

##### Flow View Source

Exploring the Flow Blockchain

<div>

flow-view-source.com

</div>

</div>

</div>

<div>

</div>

> **Metadata in Dapper checkout**

<div>

</div>

<div>

For transactions that are eligible for Dapper checkout, Dapper will
display purchase details on the checkout UI that is pulled from on-chain
NFT metadata. Using arguments to the \'**buy**\' transaction, such as
the `NFTStorefront.Listing.id`, Dapper will query the chain for metadata
related to the sale such as NFT name, description, imageURL (url to
off-chain image of NFT), and sale price.

</div>

<div>

</div>

<div>

For example, the data Dapper references for the purchase of a Genies NFT
is:

</div>

<div>

    pub struct PurchaseData {
        pub let id: UInt64
        pub let name: String
        pub let amount: UFix64
        pub let description: String
        pub let imageURL: String
        pub let paymentVaultTypeID: Type

        init(id: UInt64, name: String, amount: UFix64, description: String, imageURL: String, paymentVaultTypeID: Type) {
            self.id = id
            self.name = name
            self.amount = amount
            self.description = description
            self.imageURL = imageURL
            self.paymentVaultTypeID = paymentVaultTypeID
        }
    }

</div>

<div>

</div>

## 12 -- Flow contract examples

<div>

</div>

> **Generic contracts**

<div>

</div>

<div>

</div>

<div>

**NFT Storefront**

</div>

<div>

<div>

##### nft-storefront/contracts at main · onflow/nft-storefront

A general-purpose Cadence contract for trading NFTs on Flow -
nft-storefront/contracts at main · onflow/nft-storefront

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

**Non Fungible Token**

</div>

<div>

<div>

##### flow-nft/contracts at master · onflow/flow-nft

The Non-Fungible Token standard on the Flow Blockchain -
flow-nft/contracts at master · onflow/flow-nft

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

**Fungible Token**

</div>

<div>

<div>

##### flow-ft/contracts at master · onflow/flow-ft

The Fungible Token standard on the Flow Blockchain - flow-ft/contracts
at master · onflow/flow-ft

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

> **Specific marketplace contracts**

<div>

</div>

<div>

</div>

<div>

**NBA Top Shot**

</div>

<div>

<div>

##### nba-smart-contracts/contracts at master · dapperlabs/nba-smart-contracts

Smart contracts and transactions for Topshot, the official NBA digital
collectibles game on the Flow Blockchain - nba-smart-contracts/contracts
at master · dapperlabs/nba-smart-contracts

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

**Genies**

</div>

<div>

<div>

##### Genies on Flow

Genies is partnering with Dapper Labs to build out the Genies Avatar
Wearable NFT Marketplace on Flow, a next-generation blockchain chosen
for its unique combination of scalability, usability, and
environmentally sustainable features.

<div>

<div>

</div>

flow.com

</div>

</div>

<div>

</div>

</div>

<div>

</div>

<div>

**Kitty Items**

</div>

<div>

<div>

##### kitty-items/cadence/contracts at master · onflow/kitty-items

Based on CryptoKitties, Kitty Items is an example of a full-stack dapp
built on Flow. - kitty-items/cadence/contracts at master ·
onflow/kitty-items

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

**Ballmart**

</div>

<div>

<div>

##### Ballmart - NFT marketplace

We have the biggest balls

<div>

<div>

</div>

dapper-ballmart-git-staging-dapperlabs.vercel.app

</div>

</div>

</div>

## 13 -- How to provide images to Dapper

<div>

</div>

<div>

NFT images can be displayed on the Dapper Checkout if they are publicly
accessible **via HTTP**. For optimal performance, we recommend you
optimize these images for **150px X 150px** and host them on a **CDN**.

</div>

<div>

</div>

<div>

</div>

<div>

</div>

<div>

</div>

<div>

</div>

<div>

<div>

👋

</div>

<div>
