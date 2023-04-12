

# Dapper Self Custody

</div>

</div>

</div>

<div>

# Dapper Self Custody - Beta Test

<div>

Thank you for helping us test Dapper Self Custody. Built exclusively for
mobile devices, Dapper Self Custody is a great new option for quickly
connecting a digital wallet to your dApp. This wallet can store both
fungible and non-fungible tokens, facilitate frictionless transactions
to and from other Flow wallets, and grant users complete control over
the ownership and portability of their digital assets.

</div>

<div>

</div>

<div>

***Some important notes on the Dapper Self Custody beta test:***

</div>

-   **Testnet -** During the closed beta, Dapper Self Custody is only
    functional on Testnet.
-   **Backup and Recovery -** Backup and Recovery is still in active
    development. It is enabled on iOS and disabled on Android.
    Subsequent updates will evolve backup and recovery and enable it for
    Android. Please keep in mind that all wallets created in the beta
    phase are on Testnet.

# Getting Started

## **Step 1 - Sign Up for the Beta**

<div>

If you haven't already done so, sign up for the Dapper Self Custody beta
here:

</div>

<div>

Dapper Self Custody Beta

</div>

## **Step 2 - Install Dapper Self Custody**

<div>

Once invited, you should receive an invite e-mail from either
**TestFlight (iOS)** or **Firebase (Android)**. Accept this invite and
follow its instructions to install Dapper Self Custody on your mobile
device.

</div>

## **Step 3 - Create Your Wallet**

<div>

In Dapper Self Custody, log in to your Dapper account and follow the
prompt to create a new self custody wallet on Testnet.

</div>

## **Step 4 -** **Play Flow Rider**

<div>

</div>

<div>

We've created a sample dApp called Flow Rider to show you how Dapper
Self Custody works. Now that your wallet is ready, let's collect some
tokens!

</div>

-   **Go to** **Flow Rider** **using your desktop's web browser**
-   **Connect to Flow Rider -** Connect to Flow Rider using the Dapper
    Self Custody app by clicking the Connect to dApp button. Scan Flow
    Rider's presented QR code with Dapper Self Custody.

<div>

</div>

-   **Fund Wallet -** Once connected, follow the instructions within
    Flow Rider to fund your new self custody wallet with FLOW if you
    haven't already done so.
-   **Sign Transactions -** While you're playing Flow Rider, you'll be
    presented with pop-up screens in Dapper Self Custody asking you to
    sign transactions. Confirm these transactions inside the Dapper Self
    Custody app.

<div>

</div>

-   **Earn Flow Rider Gems as you play -** Your Flow Rider Gem count is
    reflected in both Flow Rider and Dapper Self Custody. At the bottom
    of Flow Rider, you're able to "spend" these gems exemplifying a
    token purchase transaction.

## **Step 5 - Give Feedback**

<div>

While testing Dapper Self Custody, you can take a screenshot within the
app---this will bring up the "Need help?" screen where you can report a
bug, ask a question, or suggest an improvement. Feedback given through
this in-app prompt goes straight to the team building Dapper Self
Custody. We're incredibly excited to evolve Dapper Self Custody beyond
this simple starting point and your feedback is incredibly valuable on
that journey.

</div>

<div>

</div>

<div>

</div>

# Developing for Dapper Self Custody

<div>

Dapper Self Custody uses WalletConnect 2.0 to form connection between a
dApp and the wallet. Once a connection is formed, the dApp is able to
inspect the wallet's public address and request a transaction to be
signed by the wallet. Building for Dapper Self Custody requires a dApp
to integrate WalletConnect 2.0 via FCL. The following sections will get
you started building WalletConnect 2.0 support into your Flow dApp.

</div>

## Create WalletConnect Project

<div>

Before implementing WalletConnect 2.0 into your project, you first need
to obtain a WalletConnect ***projectID***. First visit the WalletConnect
Cloud Registry:

</div>

<div>

**WalletConnect Cloud Registry**

</div>

<div>

</div>

<div>

Once logged in, select New Project.

</div>

<div>

</div>

<div>

Add details for your new WalletConnect project:

</div>

<div>

</div>

<div>

This will give you a ***projectID*** for WalletConnect which you'll
utilize in your project.

</div>

## Integrate FCL, WalletConnect

<div>

Now that you've created your WalletConnect project and have your
***projectID,*** follow the instructions below to add support for FCL
compatible wallets that use the WalletConnect 2.0 protocol - which
includes Dapper Self Custody.

</div>

<div>

Add FCL Support for WalletConnect 2.0

</div>

## Additional Resources

### Example dApp

<div>

The following Github repo is a React project that implement FCL
WalletConnect as described above. It exemplifies how to connect to a
WalletConnect supported Flow wallet like Dapper Self Custody and how to
send a transaction request.

</div>

<div>

flow-walletconnect-v2-react-dapp

</div>

<div>

</div>

<div>

The above sample dApp can be tested here:

</div>

<div>

https://fcl-walletconnect.vercel.app/

</div>

### WalletConnect 2.0

<div>

More information on WalletConnect 2.0 can be found on WalletConnect's
documentation here:

</div>

<div>

WalletConnect 2.0

</div>

<div>

</div>

<div>

<div>

![](data:image/svg+xml;base64,PHN2Zz48Zz48cGF0aD48L3BhdGg+PC9nPjwvc3ZnPg==)

</div>

<div>

TOS

</div>

</div>

<div>

</div>

</div>

</div>

</div>
