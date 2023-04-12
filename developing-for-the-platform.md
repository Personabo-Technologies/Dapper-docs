
# Developing Your Application for the Dapper Platform

<div>

Welcome to the development journey on the Dapper Platform, where your
application will reach our ever-growing user base. You can think of this
process as similar to publishing an application to an app store,
reaching eager end-users in the ecosystem. This section will walk you
through the journey of getting your application configured in the
**Testnet** environment.

</div>

## Prerequisites

<div>

Before proceeding, you will need the following:

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Access to the Developers Portal in the staging environment and access to
the organization address under the **Payout & report** section.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Deployment of your NFT smart contract to Testnet

</div>

</div>

</div>

<div>

<div>

💡

</div>

<div>

If you don't have your contract ready yet, follow the ****Testnet
Deployment Guidelines**** **** to deploy your NFT smart contract.

</div>

</div>

# Upload Your Contract to Flow NFT Catalog

<div>

FLOW NFT Catalog is an on-chain catalog of NFTs in the Flow ecosystem,
meant to provide discoverability and interoperability. It does so by
enforcing the implementation of the NonFungibleToken interface and the
metadata-views. Adding your contract here will enable other developers
and users to discover and query your NFT on the collection level.

</div>

## Prepare Your Contract

<div>

Make sure your smart contract meets the following criteria:

</div>

1.  It implements the NonFunigbleToken interfaces in the Flow
    NonFungibleToken Contract. Refer to the Example NFT for example
    implementation on how to set up collection, minter, and more.
2.  It implements the MetadataView.Resolver interface. Refer to the
    Example NFT for example implementation on MetadataViews.
3.  The smart contract itself should avoid dealing with any fungible
    token. Transactions are to be used when dealing with fungible tokens
    and other currencies.

## Upload Your contract to Flow NFT Catalog

<div>

To submit to the catalog, follow the **Submitting a Collection to the
NFT Catalog steps****.** It typically takes a couple of days for your
contract to get approved. You can check out the status of the proposal
here.

</div>

<div>

While waiting for approval, you may jump to the next step and create a
new entry for your app in the Developers Portal.

</div>

<div>

<div>

❗

</div>

<div>

You can now import an app from the NFT Catalog to the developers portal
directly, provided your contract is already uploaded to the Catalog.

</div>

</div>

# Create a New App in the Developer Portal

## Creating a New App

<div>

Login to your developers portal dashboard home page. Click the plus
button next to "Manage Apps"

</div>

<div>

</div>

<div>

After entering your app name, you will be taken to the **Manage App**
page to set up the configurations.

</div>

## Configure Application Details

<div>

The Dapper Platform will display your application details and branding
for various use-cases. In order to do that, we need details on your
application. On the **Manage App** page, you will be asked to provide
the following information:

</div>

-   **Primary website link**
-   **Description**
-   **Supported domains**
-   **Upload logos**

1.  To best ensure visibility on the inventory page, the background
    should be transparent, image cropped to the edges of the text.
2.  Image size limited to 100KB

**Upload banner**

<div>

This banner image will show up on your user login/sign-up modal. The
image should be less than 450px in width and less than 1M in size.

</div>

**Customer support link**

<div>

Users will be directed to this URL when clicking on the "Contact
Support" link on the NFT receipt.

</div>

**Twitter profile (optional)**

<div>

Your NFT Twitter profile URL

</div>

**Discord link (optional)**

<div>

Your NFT discord channel URL

</div>

## Configure NFT Inventory

<div>

In order for the Dapper Platform to be aware of your NFTs and be able to
display them in the end-user's wallet, you must register your contract.
Click on the "Add a new NFT Contract" button in the "NFT inventory
configuration" section. Fill in the form to register your NFT contract
against Dapper.

</div>

-   **NFT Contract Name**
-   **Display Name**
-   **NFT collection public path**
-   **NFT collection storage path**

<div>

<div>

❗

</div>

<div>

NFT Catalog integration in the Developer Dashboard automatically
registers your contract based on your application's Catalog Id.

</div>

</div>

## Next Step

<div>

Now that you've configured your app, the next step would be creating the
transactions using Cadence. You can either write customized
transactions, or leverage Flow NFT Catalog to generate some of the most
common transactions with the next step, Generate and Upload Transactions
and Scripts.

</div>

<div>

<div>

❗

</div>

<div>

Generating transactions **will require** your contract to be approved by
the Flow NFT Catalog first.

</div>

</div>

# Generate and Upload Transactions and Scripts

<div>

One of Dapper Platform's values is trust for the end-user. To ensure
that, transactions that your application will invoke must be
pre-registered with the platform and reviewed. In the staging
environment, custom transactions are automatically approved, whereas
developer-generated purchase transactions (exchange of funds) need to be
submitted for review.

</div>

<div>

An alternative way to generate transactions is by copying the
transactions generated by the NFT catalog. It is able to do so because
the catalog enforces your NFT contract to follow the NonFungibleToken as
well as the MetadataViews standards, allowing for further
inter-operability using the NFT Storefront contract mentioned below.
**This document will walk you through the process of using transactions
generated from the NFT Catalog.**

</div>

<div>

<div>

❗

</div>

<div>

Coming Soon: NFT Catalog integration in the Developer Dashboard will
automatically pull in NFT Catalog-generated transactions for your
application and approve them.

</div>

</div>

## Understand NFT Storefront

<div>

Dapper and the NFT Catalog leverage the NFT Storefront contract to
provide the capabilities of listing and purchasing of NFTs. It provides
a secure way to trade any NFT with any type of currency and fungible
tokens. You can think of it as a vending machine for a given account.

</div>

-   The action of "loading" the vending machine with NFT items an
    account owns is called `listing`.
-   The action of locating a listed NFT item and acquiring the item is
    called `purchasing`.

<div>

To learn more about the specifics of how the NFT Storefront works,
please visit its Flow documentation page. This is completely optional.

</div>

## Generate Required Transactions and Scripts

<div>

Once your contract has been approved, you can go to the Flow NFT Catalog
Generate Transactions (Testnet) page:

</div>

1.  Enter the unique collection identifier that was registered for your
    application.
2.  Switch the token type with the Fungible Token dropdown (it's only
    relevant to the purchase and listing transactions)
3.  Clicking on the items on the left menu bar to generate transactions
    and scripts.

<div>

Here's the list of transactions and scripts that can be generated to
meet the basic sales need of your NFT:

</div>

### Transactions:

<div>

Depending on your application's use case, you may need up to 7
transactions. Here are the different types:

</div>

-   **CollectionInitialization**
-   **DapperCreateListing**
-   **StorefrontRemoveItem**
-   **DapperBuyNFTDirect (x2)**
-   **DapperBuyNFTMarketplace (x2)**

### Scripts

-   **DapperGetPrimaryListingMetadata**
-   **DapperGetSecondaryListingMetadata**

<div>

Copy these transactions into your code and make sure to invoke them
verbatim.

</div>

## Upload Transactions and Submit for Review

<div>

To register them in the Dapper Platform, go to the developers portal
dashboard and click on the app you created, you will land on the Manage
app page. Scroll down to the "Transactions" section and click "Add new
transaction".

</div>

<div>

</div>

-   **Title**

-   **Description**

-   **Transaction type**

-   -   Purchase: Purchase transactions are the transactions your users
        sign to purchase an NFT item. This applies to both direct sales
        (**DapperBuyNFTDirect)** and p2p (**DapperBuyNFTMarketplace)**
        sales as mentioned in the catalog transaction section.
    -   Custom: Any transaction that is not a purchase transaction is
        categorized as custom transaction.

-   **Code**

-   **Metadata script(when purchase transaction type selected)**

### Step 1: Upload the direct sale transaction and script

1.  Add a new transaction
2.  Fill in the title and description, and enter something to indicate
    the fungible token this transaction uses (Dapper Utility Coin or
    Flow Utility Token)
3.  **Select "Purchase" as the transaction type**
4.  For "Code", upload **DapperBuyNFTDirect** generated with **Dapper
    Utility Coin**
5.  For "Metadata Script", paste the content of
    **DapperGetPrimaryListingMetadata**
6.  Repeat 1-5 with **DapperBuyNFTDirect** generated with **Flow Utility
    Token** using the same metadata script.

### Step 2: Upload the p2p marketplace sale transaction and script

1.  Add a new transaction
2.  Fill in title and description, enter something to indicate the
    fungible token this transaction uses (Dapper Utility Coin or Flow
    Utility Token)
3.  **Select "Purchase" as transaction type**
4.  For "Code", upload **DapperBuyNFTMarketplace**
5.  For "Metadata Script", paste the content of
    **DapperGetSecondaryListingMetadata**
6.  Repeat 1-5 with **DapperBuyNFTMarketplace** generated with **Flow
    Utility Token** using the same metadata script.

### Step 3: Upload the custom transactions

<div>

For **CollectionInitialization, DapperCreateListing** and
**StorefrontRemoveItem**, choose "Custom" as the transaction type and
upload the cdc files. Custom transactions are automatically approved.

</div>

## What's next

<div>

With the exception of custom transactions in Staging, all transactions
require a review by Dapper solution engineers in order for them to be
whitelisted and invokable. After uploading, reach out to your account
manager for review which will take up to 2 days. You can view the
approval status on the Developer Portal. It will show one of the three
statuses:

</div>

-   Live
-   In review
-   Rejected

# Testing Your Application

<div>

With the app details filled, contract registered, transactions uploaded
and reviewed, you are now ready for testing! Just to be safe, here is a
checklist of the common things to test for.

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Required application detail fields all populated.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Your contract is on the NFT Catalog

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Your contract is registered in the Developer Portal

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Transactions & Metadata scripts are copied from the NFT Catalog and
configured in your application's FCL invocations.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Transactions & Metadata scripts are copied from the NFT Catalog and
registered in the Developer Portal to be reviewed.

</div>

</div>

<div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

The **DapperGetPrimaryListingMetadata** script is part of
**DapperBuyNFTDirect**, both DUC & FUT instances.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

**DapperGetSecondaryListingMetadata** is part of
**DapperBuyNFTMarketplace**, both DUC and FUT instances.

</div>

</div>

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Transactions have been submitted and approved. Please note that custom
transactions in the Testnet environment are whitelisted by default.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Sign up for a Dapper Wallet to use as the purchaser to simulate a
purchase transaction

</div>

</div>

<div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Ensure NFT is transferred from the original (minting) account to the
purchaser\'s wallet.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Ensure the right funds were deducted from the purchaser\'s wallet.

</div>

</div>

</div>

<div>

<div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48cGF0aD48L3BhdGg+PC9zdmc+)

</div>

</div>

<div>

Ensure that the NFT image and metadata are displaying as intended.

</div>

</div>

</div>

</div>

</div>

# App Submission for Review

<div>

The final step of Testnet journey is to submit your application for
review. Upon a successful review, you would be granted to develop on
Dapper Platform for Mainnet, where Dapper end-users can't wait to see
what you have to offer.

</div>

<div>

</div>

<div>

Happy Developing!

</div>

<div>

</div>

<div>

<div>

👋

</div>

<div>
