

# Beginner Sample App

<div>

The following documentation aims to educate you on building a web-based
application on Flow. It walks through the key concepts required to
create a dApp (distributed application) for Flow with code snippets.

</div>

<div>

</div>

<div>

</div>

<div>

</div>

<div>

**Monster Maker** is a web-based dApp (distributed application) that
allows users to connect a wallet, sign a transaction to mint an NFT (a
monster) and display their collection of NFTs (their monsters) within
the app. It's meant to be a lightweight sample project to exemplify how
to build a web-based Flow project. If you are looking to build a
web-based application for Flow, exploring the Monster Maker code base
first or even building off of it is a great way to get started.

</div>

<div>

You can see a fully working demo of MonsterMaker at:

</div>

<div>

<div>

💻

</div>

<div>

https://monster-maker-web-client.vercel.app/

</div>

</div>

<div>

</div>

<div>

Github Repository:

</div>

<div>

<div>

📃

</div>

<div>

https://github.com/onflow/monster-maker

</div>

</div>

<div>

</div>

<div>

The project is divided into front-end and back-end. The front-end
(web-client) provides the user interface, and the back-end (server)
provides the minting service. Both the front-end and back-end are built
using next-js.

</div>

<div>

</div>

<div>

<div>

👉

</div>

<div>

*There is also an iOS project for Monster Maker. If you want to build a
mobile native Flow application, you can review the documentation*
*here**.*

</div>

</div>

## Run the Project

<div>

To start the front-end, open another terminal and from the command
prompt run:

</div>

<div>

    > cd web-client
    > cp .env.example .env.local
    > yarn install
    > yarn dev

</div>

<div>

<div>

👉

</div>

<div>

*For simplicity, you can use the deployed version of the monster-maker
server, which is default in* *`.env.example`**. In order for the server
to support minting transactions, a private key is required. This key is
generated in the process of creating a FLOW account where your contracts
are deployed. Instructions can be found in* *Adding Contracts to a Flow
Account**. The private key generated there can be copied into the server
side* *`.env.local`* *file. You will then be able to run your own
monster maker server locally.*

</div>

</div>

<div>

</div>

<div>

Go to `http://localhost:3001`.

</div>

<div>

</div>

## Connecting to a Wallet

<div>

Click Connect and select a wallet to use with MonsterMaker (note:
MonsterMaker is configured by default to work with `testnet`).

</div>

<div>

</div>

<div>

<div>

👉

</div>

<div>

**Flow Environments:** `testnet` is a flow network that can be used to
test flow code before deploying it into the production environment
`mainnet`. It works exactly like mainnet, but you can get flow token for
free to execute transactions through a "faucet" that transfers flow
tokens to your wallet. You can also run a flow network locally using an
emulator.

</div>

</div>

<div>

In order for a dApp to interact with the FLOW blockchain, a connection
to a user's wallet is required. All wallet and blockchain interaction is
managed through the FCL (Flow Client Library).

</div>

<div>

All Web3 related connection and execution calls will be provided through
a React context provider, and accessed through a React hook. This allows
all FCL related code to be located in one spot that components can use
conveniently.

</div>

<div>

Wallet Connection is configured using the following code:

</div>

<div>

    const {
      flowNetwork,
      accessApi,
      walletDiscovery,
      walletDiscoveryApi,
      walletDiscoveryInclude,
      addresses,
    } = NETWORK;
    const iconUrl = window.location.origin + '/images/wallet-icon.png';
    const appTitle = process.env.NEXT_PUBLIC_APP_NAME || 'MonsterMaker';

    fcl.config({
      'app.detail.title': appTitle,
      'app.detail.icon': iconUrl,
      'accessNode.api': accessApi, // connect to Flow
      'flow.network': flowNetwork,
      'discovery.wallet': walletDiscovery, // use wallets on public discovery
      'discovery.authn.endpoint': walletDiscoveryApi, // public discovery api endpoint
      'discovery.authn.include': walletDiscoveryInclude, // opt-in wallets
      '0xFungibleToken': addresses.FungibleToken, // fcl replaces alias with network address
      '0xFlowToken': addresses.FlowToken,
      '0xNonFungibleToken': addresses.NonFungibleToken,
      '0xMetadataViews': addresses.MetadataViews,
      '0xMonsterMaker': addresses.MonsterMaker,
    });

web-client/contexts/Web3.tsx (Code)

</div>

<div>

This code is part of the `Web3ContextProvider` (See the full code). This
component manages the Web3 connection via FCL and provides global access
to all web3 information to the application, such as the user,
transaction and wrapper functions to execute transactions/scripts on the
blockchain. When the application is loaded, the `Web3ContextProvider`
runs `fcl.config` to ensure that the connection to the appropriate flow
network is ready. Components can then access all flow related
functionality through the `useWeb3Context` hook.

</div>

<div>

The `network` object in the code above is derived from a constant that
defines configuration parameters for each network:

</div>

<div>

    const FLOW_ENV = process.env.NEXT_PUBLIC_FLOW_ENV || 'testnet';

    const NETWORKS = {
      emulator: {
        flowNetwork: 'local',
        accessApi: process.env.NEXT_PUBLIC_EMULATOR_API || 'http://localhost:8888',
        walletDiscovery: 'https://fcl-discovery.onflow.org/local/authn',
        walletDiscoveryApi: 'https://fcl-discovery.onflow.org/api/local/authn',
        walletDiscoveryInclude: [],
        addresses: {
          FlowToken: '0x0ae53cb6e3f42a79',
          NonFungibleToken: '0x0ae53cb6e3f42a79',
          MetadataViews: '0x0ae53cb6e3f42a79',
          MonsterMaker: '0x0ae53cb6e3f42a79',
          FungibleToken: '0xee82856bf20e2aa6',
        },
      },
      testnet: {
        flowNetwork: 'testnet',
        accessApi: 'https://rest-testnet.onflow.org',
        walletDiscovery: 'https://fcl-discovery.onflow.org/testnet/authn',
        walletDiscoveryApi: 'https://fcl-discovery.onflow.org/api/testnet/authn',
        walletDiscoveryInclude: [
          '0x82ec283f88a62e65', // Dapper Wallet
        ],
        addresses: {
          FlowToken: '0x7e60df042a9c0868',
          NonFungibleToken: '0x631e88ae7f1d7c20',
          MetadataViews: '0x631e88ae7f1d7c20',
          MonsterMaker: '0xfd3d8fe2c8056370',
          FungibleToken: '0x9a0766d93b6608b7',
        },
      },
      mainnet: {
        flowNetwork: 'mainnet',
        accessApi: 'https://rest-mainnet.onflow.org',
        walletDiscovery: 'https://fcl-discovery.onflow.org/authn',
        walletDiscoveryApi: 'https://fcl-discovery.onflow.org/api/authn',
        walletDiscoveryInclude: [
          '0xead892083b3e2c6c', // Dapper Wallet
        ],
        addresses: {
          FlowToken: '0x1654653399040a61',
          NonFungibleToken: '0x1d7e57aa55817448',
          MetadataViews: '0x1d7e57aa55817448',
          MonsterMaker: '',
          FungibleToken: '0xf233dcee88fe0abe',
        },
      },
    } as const;

    type NetworksKey = keyof typeof NETWORKS;

    export const NETWORK = NETWORKS[FLOW_ENV as NetworksKey];

web-client/constants/networks.js (Code)

</div>

<div>

<div>

👉

</div>

<div>

Wallet discovery is most easily done by using FCL's discovery service.
This is simply a url that provides a list of approved Flow wallets a
user can select to authenticate (e.g.
`https://fcl-discovery.onflow.org/testnet/authn`). **Note:** There is
also another way of connecting to wallets via the **WalletConnect**
protocol. This is a chain agnostic protocol that allows a user to select
a wallet from a list of wallets or scan a QR code to launch their wallet
from their smartphone. WalletConnect can be added the list of wallets
presented as part of wallet discovery. Code required for adding
WalletConnect can be seen here.

</div>

</div>

<div>

<div>

💡

</div>

<div>

**Important:** In order to include Dapper Wallet, it must be included in
the config as part of the `walletDiscoveryInclude` with the appropriate
address for the flow environment as shown above. **Testnet:**
0x82ec283f88a62e65 **Mainnet:** 0xead892083b3e2c6c For more information
see the FLOW documentation.

</div>

</div>

<div>

</div>

<div>

The following function is provided for connecting the wallet through
`fcl.authenticate`

</div>

<div>

    const connect = useCallback(() => {
      fcl.authenticate();
    }, []);

web-client/contexts/Web3.tsx (Code)

</div>

<div>

</div>

<div>

Which can be called as follows:

</div>

<div>

    const { connect } = useWeb3Context();

    // ...
    <Button
      src="/images/ui/connect_button.png"
      width={576}
      height={208}
      onClick={connect}
      alt="Connect wallet"
    />
    // ...

web-client/pages/index.tsx (Code)

</div>

## Executing a Script

<div>

After connecting your chosen wallet, you are presented with the
Initialize screen (if you haven't initialized previously). Initializing
will ensure that your wallet has a MonsterMaker Collection that can be
used to transfer a MonsterMaker NFT into your wallet. All NFTs have to
exist within a collection - if your wallet does not have the required
collection, then any transfer of NFTs to your wallet will fail.

</div>

<div>

</div>

<div>

</div>

<div>

To check whether or not a wallet has been initialized with a
MonsterMaker Collection, a Cadence script is used to check if the user
has a MonsterMaker Collection in their wallet.

</div>

<div>

A script on the FLOW blockchain is code that performs a read only
transaction and does not modify the blockchain. In our web applications,
scripts are Cadence code that are represented as strings that will be
passed to the `fcl.query` function:

</div>

<div>

    const isInitialized = `
    import NonFungibleToken from 0xNonFungibleToken
    import MonsterMaker from 0xMonsterMaker

    pub fun main(address: Address) : Bool {
        let account = getAccount(address)

        let vaultRef = account
        .getCapability<&{NonFungibleToken.CollectionPublic}>(MonsterMaker.CollectionPublicPath)
        .check()

        return vaultRef
    }
    `;

    export default isInitalized;

web-client/cadence/scripts/isInitialized.ts

</div>

<div>

<div>

💡

</div>

<div>

The code above takes a wallet address, gets the account from the
address, and then looks for a reference to a MonsterMaker Collection. If
the MonsterMaker Collection exists, `true` is returned otherwise
`false`.

</div>

</div>

<div>

</div>

<div>

In MonsterMaker, calling a script is done using the `executeScript`
wrapper function. Here we check if the user has a MonsterMaker
Collection initialized, and we redirect accordingly:

</div>

<div>

    import isInitializedScript from 'cadence/scripts/isInitialized';

    // ...
    const [isInitialized, setIsInitialized] = useState<boolean | null>(null);

    const { connect, user, executeScript } = useWeb3Context();

    const checkIsInitialized = async () => {
      try {
        const res: boolean = await executeScript(
          isInitalizedScript,
          (arg: any, t: any) => [arg(user.addr, t.Address)],
        );

        setIsInitialized(res);
      } catch (error) {
        console.error(error);
      }
    };

    useEffect(() => {
      if (user.loggedIn) {
        checkIsInitialized();
      }
    }, [user]);

    useEffect(() => {
      if (user.loggedIn && isInitialized === false) {
        router.push(ROUTES.INITIALIZE);
      } else if (user.loggedIn && isInitialized === true) {
        router.push(ROUTES.CREATE);
      }
    }, [user, router, isInitialized]);
    // ...

web-client/pages/index.tsx (Code)

</div>

<div>

Behind the scenes, `executeScript` is just a wrapper defined for
convenience in the `Web3ContextProvider` that is calling `fcl.query`. If
you wanted, you could call `fcl.query` from right within your
components.

</div>

<div>

    const executeScript = useCallback(async (cadence: string, args: any = () => []) => {
      try {
        const res: any = await fcl.query({
          cadence: cadence,
          args,
        });
        return res;
      } catch (error) {
        console.error(error);
      }
    }, []);

web-client/contexts/Web3.tsx (Code)

</div>

<div>

If the wallet does not have a MonsterMaker Collection, then we will have
to initialize the account through a Cadence transaction, shown in the
next section.

</div>

## Executing a Transaction

<div>

MonsterMaker has two transactions - the initialize transaction and
minting transaction. Transactions modify the Flow Blockchain and require
approval from the user before they are executed, as well as some gas in
order to execute (gas is a small amount of Flow Token required to pay
for the cost of executing the transaction). Executing transactions is
very similar to scripts, but a transaction will go through different
statuses and finally be SEALED if the transaction was successful.

</div>

<div>

<div>

👉

</div>

<div>

Flow Transactions can be called from the client or server side, but it
doesn't matter where a transaction is called from.

</div>

</div>

<div>

<div>

⚠️

</div>

<div>

In the case of Dapper Wallet, before transactions can be executed they
must whitelisted on the Dapper Developer Dashboard. This is done to
ensure that users of the Dapper Wallet can trust that the transactions
executed through the Dapper Wallet are safe and have been approved by
Dapper. More details on adding transactions to the Dapper Platform in
the next section.

</div>

</div>

<div>

</div>

<div>

Like scripts, transactions are Cadence code that are stored as strings
that will be passed to the `fcl.mutate` function:

</div>

<div>

    const initAccount = `
        import NonFungibleToken from 0xNonFungibleToken
        import MonsterMaker from 0xMonsterMaker
        import MetadataViews from 0xMetadataViews

        transaction {
            prepare(signer: AuthAccount) {
                // if the account doesn't already have a collection
                if signer.borrow<&MonsterMaker.Collection>(from: MonsterMaker.CollectionStoragePath) == nil {

                    // create a new empty collection
                    let collection <- MonsterMaker.createEmptyCollection()
                    
                    // save it to the account
                    signer.save(<-collection, to: MonsterMaker.CollectionStoragePath)

                    // create a public capability for the collection
                    signer.link<&MonsterMaker.Collection{NonFungibleToken.CollectionPublic, MonsterMaker.MonsterMakerCollectionPublic, MetadataViews.ResolverCollection}>(MonsterMaker.CollectionPublicPath, target: MonsterMaker.CollectionStoragePath)
                }
            }
        }
    `;

    export default initAccount;

web-client/cadence/transactions/initAccount

</div>

<div>

<div>

💡

</div>

<div>

The code above will save a MonsterMaker Collection to the user's wallet.
The signer is the user. We first check the account for an existing
MonsterMaker Collection, if it doesn't exist, we create an empty
MonsterMaker Collection, and save it to the account.

</div>

</div>

<div>

</div>

<div>

Calling a transaction is simple:

</div>

<div>

    const { executeTransaction, transaction } = useWeb3Context();

    const handleInit = async () => {
      await executeTransaction(initAccountTxn, () => [], {
        limit: 9999,
      });
    };

    useEffect(() => {
      if (transaction.id !== null) {
        router.push(ROUTES.CREATE);
      }
    }, [router, transaction]);

    // ...
    <Button
      src="/images/ui/initialize_button.png"
      width={640}
      height={208}
      onClick={handleInit}
      alt="Initialize wallet"
    />
    // ...

web-client/pages/initialize.tsx (Code)

</div>

<div>

Note that here, no arguments are required for the transaction which is
why we pass a function that returns an empty array (`() ⇒ []`), and in
order to ensure that the transaction executes, we pass in an option with
a gas `limit` set to `9999`. If the limit is a lower value, for example
`10`, then if the transaction costs `50`, it will fail. Limits allow us
to set a ceiling on how much gas we are willing to pay for a
transaction.

</div>

<div>

</div>

<div>

Behind the scenes, `executeTransaction` is a wrapper function for
`fcl.mutate` created for convenience in `Web3ContextProvider`:

</div>

<div>

    const executeTransaction = useCallback(
        async (cadence: string, args: any = () => [], options: any = {}) => {
          setTransactionInProgress(true);
          setTransactionStatus(-1);

          const transactionId = await fcl
            .mutate({
              cadence,
              args,
              payer: fcl.authz,
              proposer: fcl.authz,
              authorizations: [fcl.authz],
              limit: options.limit || 50,
            })
            .catch((e: Error) => {
              setTransactionInProgress(false);
              setTransactionStatus(500);
              setTransactionError(String(e));
            });

          if (transactionId) {
            setTxId(transactionId);
            fcl.tx(transactionId).subscribe((res: any) => {
              setTransactionStatus(res.status);
              setTransactionInProgress(false);
            });
          }
        },
        [],
      );

web-client/contexts/Web3.tsx (Code)

</div>

<div>

<div>

👉

</div>

<div>

`fcl.query` is used for calling a script (ie cadence code that "queries"
the blockchain), and `fcl.mutate` is used for calling a transaction (ie
cadence code that "mutates" the blockchain).

</div>

</div>

<div>

</div>

<div>

Note that while the `initAccountTxn` does not require any arguments,
arguments can be passed into a transaction. In order to pass arguments
to a cadence transaction, they are passed using a function that returns
an array of arguments as follows:

</div>

<div>

    executeTransaction(
        cadenceScript, 
        (arg: any, t: any) => [
        arg('0xf3792e919674928c', t.Address),
        arg(1234, t.Int),
        arg('string arg', t.String),
        // etc...
      ],
    });

</div>

<div>

</div>

<div>

After your wallet is initialized, you will be presented with the Minting
screen:

</div>

<div>

</div>

<div>

</div>

<div>

Clicking **Mint** will call the minting transaction which will execute a
Flow Transaction that will mint the NFT and transfer to your wallet
address. The minter address is required, so there will be a call to the
backend api `/api/signAsMinter`. This will return the signature of the
app created from the private key.

</div>

<div>

</div>

### Minting the MonsterMaker NFT

<div>

The following Cadence Script will mint a MonsterMaker NFT:

</div>

<div>

    const mintMonster = `
    import NonFungibleToken from 0xNonFungibleToken
    import MonsterMaker from 0xMonsterMaker
    import MetadataViews from 0xMetadataViews
    import FungibleToken from 0xFungibleToken

    // This transction uses the NFTMinter resource to mint a new NFT.
    //
    // It must be run with the account that has the minter resource
    // stored at path /storage/NFTMinter.

    transaction(
        recipient: Address, 
        background: Int,
        head: Int,
        torso: Int, 
        leg: Int
    ) {

        // local variable for storing the minter reference
        let minter: &MonsterMaker.NFTMinter

        /// Reference to the receiver's collection
        let recipientCollectionRef: &{NonFungibleToken.CollectionPublic}

        /// Previous NFT ID before the transaction executes
        let mintingIDBefore: UInt64

        prepare(signer: AuthAccount) {
            self.mintingIDBefore = MonsterMaker.totalSupply

            // Borrow a reference to the NFTMinter resource in storage
            self.minter = signer.borrow<&MonsterMaker.NFTMinter>(from: MonsterMaker.MinterStoragePath)
                ?? panic("Could not borrow a reference to the NFT minter")

            // Borrow the recipient's public NFT collection reference
            self.recipientCollectionRef = getAccount(recipient)
                .getCapability(MonsterMaker.CollectionPublicPath)
                .borrow<&{NonFungibleToken.CollectionPublic}>()
                ?? panic("Could not get receiver reference to the NFT Collection")
        }

        execute {
            let componentValue = MonsterMaker.MonsterComponent(background: background, head: head, torso: torso, leg: leg)

            // TODO: Add royalty feature to KI using beneficiaries, cuts, and descriptions. At the moment, we don't provide royalties with KI, so this will be an empty list.
            let royalties: [MetadataViews.Royalty] = []

            // mint the NFT and deposit it to the recipient's collection
            self.minter.mintNFT(
                recipient: self.recipientCollectionRef,
                component: componentValue,
                royalties: royalties
            )
        }

        post {
            self.recipientCollectionRef.getIDs().contains(self.mintingIDBefore): "The next NFT ID should have been minted and delivered"
            MonsterMaker.totalSupply == self.mintingIDBefore + 1: "The total supply should have been increased by 1"
        }
    }
    `;

    export default mintMonster;

server/cadence/transactions/mintMonster.ts

</div>

<div>

<div>

💡

</div>

<div>

The above transaction will execute in three phases, `prepare`, `execute`
and `post`. **prepare:** The transaction values are set up and a
reference to the wallet's MonsterMaker collection is retrieved.
**execute:** The MonsterMaker component is created describing the
background, head, etc the user selected, and the NFT is minted which
will transfer the newly minted NFT to the user's wallet. **post:** A
check is done to ensure that the supply has incremented correctly.

</div>

</div>

<div>

</div>

<div>

Transactions go through several statuses, from `Pending` to `Sealed`.
Once a transaction is submitted to the FLOW blockchain, is it not
officially done until it is in the Sealed status.

</div>

<div>

This code snippet will call the minting api, check for when the status
is sealed, and redirect the user once it is all complete:

</div>

<div>

    // ...
    const handleClickMint = async () => {
      setIsMintInProgress(true);

      const txId = await fcl.mutate({
        cadence: mintMonster,
        args: (arg: any, t: any) => [
          arg(backgroundSelector.index, t.Int),
          arg(headSelector.index, t.Int),
          arg(torsoSelector.index, t.Int),
          arg(legsSelector.index, t.Int),
          arg(monsterPrice, t.UFix64),
        ],
        authorizations: [fcl.currentUser, minterAuthz],
      });

      setTxId(txId);
    };

    useEffect(() => {
      if (txId) {
        fcl.tx(txId).subscribe(setTxStatus);
      }
    }, [txId]);

    useEffect(() => {
      if (txStatus?.statusString === 'SEALED') {
        router.push(ROUTES.VIEW);
      }
    }, [txStatus, router]);

    // ...

web-client/pages/create.tsx (Code)

</div>

<div>

</div>

<div>

Once the transaction is Sealed, you will be redirected to the View page
which will show the newly minted NFT, and any other NFTs you have
previously minted:

</div>

<div>

</div>

<div>

</div>

<div>

The script for viewing the NFTs is as follows:

</div>

<div>

    const getMonstersScript = `
        import NonFungibleToken from 0xNonFungibleToken
        import MetadataViews from 0xMetadataViews
        import MonsterMaker from 0xMonsterMaker

        pub struct Monster {
            pub let name: String
            pub let description: String
            pub let thumbnail: String
            pub let itemID: UInt64
            pub let resourceID: UInt64
            pub let owner: Address
            pub let component: MonsterMaker.MonsterComponent

            init(
                name: String,
                description: String,
                thumbnail: String,
                itemID: UInt64,
                resourceID: UInt64,
                owner: Address,
                component: MonsterMaker.MonsterComponent
            ) {
                self.name = name
                self.description = description
                self.thumbnail = thumbnail
                self.itemID = itemID
                self.resourceID = resourceID
                self.owner = owner
                self.component = component
            }
        }

        pub fun getMonsterById(address: Address, itemID: UInt64): Monster? {

            if let collection = getAccount(address).getCapability<&MonsterMaker.Collection{NonFungibleToken.CollectionPublic, MonsterMaker.MonsterMakerCollectionPublic}>(MonsterMaker.CollectionPublicPath).borrow() {
                
                if let item = collection.borrowMonsterMaker(id: itemID) {

                    if let view = item.resolveView(Type<MetadataViews.Display>()) {

                        let display = view as! MetadataViews.Display
                        
                        let owner: Address = item.owner!.address!

                        let thumbnail = display.thumbnail as! MetadataViews.HTTPFile     

                        
                        return Monster(
                            name: display.name,
                            description: display.description,
                            thumbnail: thumbnail.url,
                            itemID: itemID,
                            resourceID: item.uuid,
                            owner: address,
                            component: item.component
                        )
                    }
                }
            }

            return nil
        }

        pub fun main(address: Address): [Monster] {
            let account = getAccount(address)
            let collectionRef = account.getCapability(MonsterMaker.CollectionPublicPath)!.borrow<&{NonFungibleToken.CollectionPublic}>()
                ?? panic("Could not borrow capability from public collection")
            
            let ids = collectionRef.getIDs()

            let monsters : [Monster] = []

            for id in ids {
                if let monster = getMonsterById(address: address, itemID: id) {
                    monsters.append(monster)
                }
            }

            return monsters
        }
    `;

    export default getMonstersScript;

web-client/cadence/scripts/getMonsters.ts

</div>

<div>

<div>

💡

</div>

<div>

This script will return an array of Monster NFTs at a user's address.
The account is retrieved from the address with the MonsterMaker
Collection. For each of the IDs we retrieve the details for the Monster
NFT and store it in the Monster struct and add to the array.

</div>

</div>

<div>

</div>

<div>

Calling the `getMonsters` script looks like this:

</div>

<div>

    useEffect(() => {
        const getMonsters = async () => {
          const res: GetMonstersResponse = await executeScript(
            getMonstersScript,
            (arg: any, t: any) => [arg(user.addr, t.Address)],
          );
          setMonsters(res || []);
        };

        getMonsters();
      }, [executeScript, user.addr]);

web-client/pages/view.tsx (Code)

</div>

<div>

</div>

<div>

If you used Dapper Wallet, you can see the transactions executed, and
the NFTs in your Inventory:

</div>

## Adding App to Dapper Platform

<div>

In order to use a dApp with the Dapper Wallet and execute transactions
in the Dapper ecosystem, there are additional steps.

</div>

<div>

Dapper Wallet requires transactions to be whitelisted to ensure that
they are safe for users to execute - this allows Dapper to maintain
quality control for the dApps that want to be part of the Dapper eco
system. The basic steps to enable a dApp to interact with Dapper Wallet
is to register an organization, create an app as part of that
organization, and add all the required transactions for that app.

</div>

<div>

The following steps provide an overview of how to add an app to Dapper
Platform. For an in depth guide see Setup with Dapper on Testnet.

</div>

1.  Gain access to the Developer Dashboard (staging / production)
    through your Customer Representative.
2.  Login to your organization on the Dapper Platform Developer
    Dashboard

<div>

</div>

1.  Add the app, in the case of this example, our app is MonsterMaker

<div>

</div>

1.  Add Information about the app

<div>

</div>

1.  Add any required contracts. In this case, we will include the
    MonsterMaker contract:

<div>

</div>

1.  Add any required transactions. Note there are two types of
    transactions: Custom and Purchase. A Custom transaction is any
    transaction that does not involve going through Dapper Wallet's
    Purchase flow. A Purchase transaction is one that will go through
    Dapper Wallet's purchase flow potentially involving a purchase via a
    user's credit card.

<div>

</div>

<div>

Note that for the Code section, you will need to ensure that the Cadence
code matches the code you are calling from the client *character for
character*, otherwise the transaction will not be supported.

</div>

<div>

Also note that the contract addresses must match for the flow
environment you are setting up (testnet/mainnet). If you are using
aliases such as `0xMonsterMaker`, `fcl` will replace with the
appropriate address before executing the transaction.

</div>

<div>

</div>

# Resources

<div>

**FCL JS**

</div>

<div>

FCL JS is the Javacript SDK for FCL. This SDK is integrated into the
Monster Maker sample.

</div>

<div>

<div>

📃

</div>

<div>

https://github.com/onflow/fcl-js

</div>

</div>

-   **FCL API Reference**
-   Flowscan on **Testnet****/****Mainnet**  - evaluate transaction
    statuses and account transactions on chain.
-   **NFT Catalog** - a catalog of registered NFTs on testnet/mainnet.

### Useful Network Addresses

<div>

+-----------------+-----------------+-----------------+-----------------+
| <div>           | <div>           | <div>           | <div>           |
|                 |                 |                 |                 |
| **Contract**    | **Emulator**    | **Testnet**     | **Mainnet**     |
|                 |                 |                 |                 |
| </div>          | </div>          | </div>          | </div>          |
+-----------------+-----------------+-----------------+-----------------+
| <div>           | <div>           | <div>           | <div>           |
|                 |                 |                 |                 |
| FungibleToken   | 0xe             | 0x9             | 0xf             |
|                 | e82856bf20e2aa6 | a0766d93b6608b7 | 233dcee88fe0abe |
| </div>          |                 |                 |                 |
|                 | </div>          | </div>          | </div>          |
+-----------------+-----------------+-----------------+-----------------+
| <div>           | <div>           | <div>           | <div>           |
|                 |                 |                 |                 |
| FlowToken       | 0x0             | 0x7             | 0x1             |
|                 | ae53cb6e3f42a79 | e60df042a9c0868 | 654653399040a61 |
| </div>          |                 |                 |                 |
|                 | </div>          | </div>          | </div>          |
+-----------------+-----------------+-----------------+-----------------+
| <div>           | <div>           | <div>           | <div>           |
|                 |                 |                 |                 |
| N               | </div>          | 0x6             | 0x1             |
| onFungibleToken |                 | 31e88ae7f1d7c20 | d7e57aa55817448 |
|                 |                 |                 |                 |
| </div>          |                 | </div>          | </div>          |
+-----------------+-----------------+-----------------+-----------------+
| <div>           | <div>           | <div>           | <div>           |
|                 |                 |                 |                 |
| MetadataViews   | </div>          | 0x6             | 0x1             |
|                 |                 | 31e88ae7f1d7c20 | d7e57aa55817448 |
| </div>          |                 |                 |                 |
|                 |                 | </div>          | </div>          |
+-----------------+-----------------+-----------------+-----------------+

</div>

<div>

See Flow Core Contracts for more information

</div>

<div>

</div>

<div>

<div>

👋

</div>

<div>
