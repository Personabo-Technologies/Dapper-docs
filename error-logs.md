
# Error Logs

<div>

<div>

</div>

<div>

Error Log \[Alpha\] provides developers visibility into purchased
transactions that were failed by the Dapper Platform. Detailed errors
are logged in the Developer Portal under each Application's page. It
allows developers to look up recent and previous errors with timestamp,
trace id, error details and the failure types. Please refer to the
`Error Code & Resolution` section below for more details.

</div>

</div>

<div>

</div>

# Usage

<div>

To reach the Error Log page, navigate to the desire application page in
the Developer portal and click on `View error log`

</div>

<div>

</div>

<div>

</div>

<div>

Each error entry contains the following fields

</div>

<div>

+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| **Name:**                         | **Description:**                  |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Time                              | Timestamp of error in local time. |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Error Code                        | The classification of error.      |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Trace ID                          | The trace identifier of the       |
|                                   | error.                            |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Details                           | A high-level description of the   |
|                                   | error.                            |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Data Details                      | Additional metadata of the        |
|                                   | transaction, ie. arguments, type, |
| </div>                            | id, etc.                          |
|                                   |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+

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

</div>

# Error Codes & Resolutions

<div>

+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| **Error Code**                    | **Description**                   |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| TXI-301                           | Missing seller address            |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| TXI-302                           | First argument not of the address |
|                                   | type                              |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| TXI-303                           | Seller address not of the Dapper  |
|                                   | Platform                          |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| TXI-901                           | Failed execution of metadata      |
|                                   | script                            |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| TXI-902                           | Incorrect return type of metadata |
|                                   | script                            |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+

</div>

## TXI-301 - Missing merchant address

<div>

On Dapper Platform, it is mandatory for the `Address` of the seller to
be the first argument for the transaction. Chances are it's not being
sent through FCL.

</div>

<div>

**Resolution:**

</div>

<div>

Append the address as an argument to the FCL mutation call:

</div>

<div>

    await fcl.mutate({
        ...
      args: (arg, t) => [
          arg("0xcd7e0dfeb71e1e8a", t.Address) // This MUST be here
            ... // other args for the Transaction
        ],
    })

</div>

<div>

</div>

## TXI-302 - First argument not of the address type

<div>

As mentioned in TX-301, the first argument of the transaction must be of
the type, `Address`.

</div>

<div>

**Resolution:**

</div>

<div>

Make sure your first argument is of type `Address` and check if its
value is a well-formatted Flow Address, starting with `0x`.

</div>

<div>

    arg("0xcd7e0dfeb71e1e8a", t.Address) // It must have the `0x` prefix.

</div>

<div>

</div>

## TXI-303 - Seller address not of the Dapper Platform

<div>

Only Dapper accounts can be used as the seller. This error occurs when a
non-Dapper account was passed-in.

</div>

<div>

**Resolution:**

</div>

<div>

Depending on whether the purchase transaction is primary (direct) or
secondary (P2P), different values are to be used.

</div>

<div>

+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| **Transaction Type**              | **Required Address**              |
|                                   |                                   |
| </div>                            | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Primary Sales                     | The merchant/organization address |
|                                   | assigned to you by Dapper. Can be |
| </div>                            | fund in the `Payout & report`     |
|                                   | section of the developer portal   |
|                                   | as indicated below.               |
|                                   |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+
| <div>                             | <div>                             |
|                                   |                                   |
| Secondary Sales                   | The Dapper account address of the |
|                                   | seller.                           |
| </div>                            |                                   |
|                                   | </div>                            |
+-----------------------------------+-----------------------------------+

</div>

<div>

</div>

<div>

</div>

## TXI-901 - Failed execution of metadata script

<div>

Something went wrong while executing the associated metadata script.
Cadence error details are located in the `Details` column of each error
entry.

</div>

<div>

**********************Resolution:**********************

</div>

<div>

Observe any clues in the `Details` section. Common mistakes include:

</div>

-   The metadata script arguments mis-matches against the corresponding
    FCL request. The number, type and order of arguments must be
    preserved.
-   Metadata script ran into an execution error. The error message is
    captured in the error log entry and indicate reason.

<div>

</div>

## TXI-902 - Incorrect return type of metadata script

<div>

The metadata script did not return the expected well-formed
`PurchaseData` structure.

</div>

<div>

**Resolution:**

</div>

<div>

Make sure following `Purchase Data` structure is returned:

</div>

<div>

    pub struct PurchaseData {
      pub let id: UInt64
      pub let name: String
      pub let amount: UFix64
      pub let description: String
      pub let imageURL: String
      pub let paymentVaultTypeID: Type
    }

</div>

<div>

The signature of the metadata script should resemble the following:

</div>

<div>

    pub fun main(sellerAddress: Address, price: UFix64, id: UInt64): PurchaseData

</div>

<div>

**Example:**

</div>

<div>

Keep in mind the arguments of the script must match the ones sent
through FCL during transaction invocation. For the above script as an
example, the corresponding transaction signature is required to be:

</div>

<div>

    transaction(sellerAddress: Address, price: UFix64, id: UInt64) 

</div>

<div>

And the corresponding FCL `mutate` call:

</div>

<div>

    await fcl.mutate({
        ...
      args: (arg, t) => [
          arg("0xcd7e0dfeb71e1e8a", t.Address),
        arg("9.99", t.UFix64),
        arg(73532, t.UInt64)
        ],
    })

</div>

<div>

</div>

<div>

</div>

# FAQ

<div>

<div>

❓

</div>

<div>

**Why doesn't a non-whitelisted transaction showing up in Error Log?**

<div>

The Dapper platform is not able to tie a non-whitelisted transaction to
an Dapp nor an Organization. The error message that you would see
through FCL is, `This transaction is not supported`.

</div>

</div>

</div>

<div>

<div>

❓

</div>

<div>

**What are the places where errors can happen?**

<div>

Errors can happen at the application level, Dapper Platform as well as
on the blockchain. Errors that originate from the application can be
observed in the application log. Errors that originate on the Dapper
Platform is surfaced in the Error Log. Errors that happen on the
blockchain can be observed through the Flowscan tool.

</div>

</div>

</div>

<div>

<div>

❓

</div>

<div>

**Does the error log show all the errors?**

<div>

Error log only show the errors that originates from Dapper platform as
we inspect the transaction.

</div>

</div>

</div>

<div>

<div>

❓

</div>

<div>

**Would I see errors being surfaced as they occur?**

<div>

At this time, you must refresh the page in order to see the latest
errors.

</div>

</div>

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
