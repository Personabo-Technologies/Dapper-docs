

# **Verify Account Ownership With a Single Click with Account Proof**

<div>

<div>

ℹ️

</div>

<div>

Using Account Proof, developers can now verify the account ownership for
a user all with a single AuthN click. No longer will there be an
additional user_signature or transaction authorization call.

</div>

</div>

<div>

An "account-proof" is a cryptographically signed message generated
during authentication to your application that proves a Dapper Wallet
user controls a specific Flow account. An account-proof can be used as
an authentication mechanism in your application.

</div>

<div>

The process of requesting and processing an account proof is as follows:

</div>

1.  configure your FCL instance with an `fcl.accountProof.resolver`
    function that fetches the `appIdentifier` and cryptographically
    random `nonce` from your backend service and sends these values to
    Dapper Wallet
2.  verify the `account-proof` data returned by Dapper Wallet against
    the user's Flow account on-chain, thereby proving control of that
    account.

<div>

For more information, please refer to Flow's Proving Authentication
documentation.

</div>

## Configure an accountProof.resolver function

<div>

To have your application receive an account-proof during authentication,
an `account-proof-resolver` must be configured in FCL. The configuration
specifics will depend upon your application's specific implementation,
but the process is the same: configure FCL with an
`account-proof-resolver` function that fetches an `appIdentifier` and
cryptographic 32-byte random `nonce` from your application's backend
**each time** a user attempts to authenticate with the wallet.

</div>

<div>

<div>

💡

</div>

<div>

See the FCL account-proof documentation for technical details and
security considerations, especially regarding the `nonce` in the
**************************************Implementation
considerations************************************** section.

</div>

</div>

<div>

An account-proof-resolver can be configured as follows:

</div>

<div>

    import {config} from "@onflow/fcl"

    type AccountProofData = {
      // e.g. "Awesome App (v0.0)" - A human readable string to identify your application during signing
      appIdentifier: string;  

      // e.g. "75f8587e5bd5f9dcc9909d0dae1f0ac5814458b2ae129620502cb936fde7120a" - minimum 32-byte random nonce as hex string
      nonce: string;          
    }

    type AccountProofDataResolver = () => Promise<AccountProofData | null>;

    config({
      "fcl.accountProof.resolver": accountProofDataResolver
    })

</div>

<div>

The resolver function **must immediately resolve the promise with data
that is available locally,** rather than fetched from a backend during
authentication so that the browser (notably Safari and Firefox) does not
block the Dapper Wallet popup. The application backend should still
generate and track the `nonce`.

</div>

<div>

<div>

💡

</div>

<div>

When your app calls `fcl.authenticate` or `fcl.logIn` in response to a
user action such as a button click, FCL will first execute the
`acccount-proof-resolver` function, then call `window.open` to open a
popup window to Dapper Wallet. On Safari and Firefox, if the delay
between when the user action was performed and when the call to
`window.open` is greater than a certain threshold (exact number unknown,
but it is very small), the browser will block the popup.

</div>

</div>

<div>

Depending on the implementation of your application, different
strategies can be used to make the `AccountProofData` available locally
in the frontend.

</div>

<div>

One of the simplest is to load the data on page load, then configure the
`account-proof-resolver` to use this local data:

</div>

<div>

    // on page load
    import {config} from "@onflow/fcl"

    const accountProofData = await API('/account-proof-data'); // => { appIdentifier, nonce }
    await config().put('fcl.accountProof.resolver', () => accountProofData);

</div>

<div>

If a framework such as NextJS is used, the data can be generated
server-side and set as Page props:

</div>

<div>

    import {config} from "@onflow/fcl"

    type Props = {
      nonce: string;
      appIdentifier: string;
    }

    export function MyPage({ nonce, appIdentifier }: Props) {
      useEffect(() => 
        config().put('fcl.accountProof.resolver', () => ({ nonce, appIdentifier }),
      [])

      return (<h1>My app</h1>)
    }

    export const getServerSideProps: GetServerSideProps = async (context) => {
      return await fetch('/api/account-proof-data');
    };

</div>

<div>

Your backend will need to store the `nonce` somehow so that it can later
be verified against the account-proof data returned during
authentication. How you store this data is up to you, but for a
stateless solution, we recommend setting a cookie, which will be sent to
your backend when verifying the account proof (this is a similar
technique to the double-submit cookie pattern using to prevent CSRF):

</div>

<div>

    // api/account-proof-data

    function accountProofData(req, res) {
      const appIdentifier = process.env.APP_IDENTIFIER;
      const nonce = crypto.randomBytes(32).toString('hex');
      const accountProofData = { appIdentifier, nonce }

      setCookie('account-proof', JSON.stringify(accountProofData), {
        maxAge: 60 * 30,
        httpOnly: true,
        sameSite: 'strict',
        secure: true
      });
      return accountProofData;
    }

</div>

## Verify the account-proof data

<div>

Once the user successfully authenticates with their Dapper Wallet, an
`account-proof` service containing the user-signed message will be
available in FCL's `currentUser` snapshot. This data should be sent to
your backend for verification. If the verification is successful, you
can proceed with the login flow, perhaps by setting a session cookie. If
the verification is not successful, then the login flow should be halted
and an error should be shown to the user:

</div>

<div>

    type AccountProofService = {
      type: 'account-proof';
      method: 'DATA';
      data: {
        address: string;
        nonce: string;
        signatures: { addr: string; keyId: number; signature: string }[]
      }
    }

    // verify the account proof
    fcl.currentUser().subscribe(async (user) => {
      const accountProof: AccountProofService = user.services?.find((s) => s.type === 'account-proof');

      if (accountProof) {
        const { verified } = await verifyAccountProof(accountProof);
        if (verified) {
          // account-proof is verified. Proceed with login flow
        } else {
          // account-proof is not verified. Stop login flow
        }
      }
    }

    async function verifyAccountProof(accountProof) {
      const response = await fetch('/api/verify-account-proof', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(accountProof),
      });
      return await response.json();
    }

</div>

<div>

To verify the account-proof in your application's backend service:

</div>

<div>

    import { AppUtils } from '@onflow/fcl';

    export default async function verifyAccountProof(req, res) {
      try {
        const { data } = req.body;
        const { address, nonce, signatures } = data || {};
        const appIdentifier = process.env.APP_IDENTIFIER

        // get the stored nonce. If using the double-submit cookie pattern,
        // get the nonce from the cookie.
        const accountProofCookie = JSON.parse(getCookie('account-proof'));
            
        if(accountProofCookie.nonce !== nonce
          || accountProofCookie.appIdentifier !== appIdentifier) {
          res.status(401).end();
          return;
        } 
                
        const accountProofData = {
          address,
          nonce,
          signatures,
        };

        verified = await AppUtils.verifyAccountProof(
          appIdentifier,
          accountProofData,
        );
        
        res.status(200).json({ verified });
      } catch (err: any) {
        console.log(err);
        res.status(401).json({ error: err.message });
      }
    }

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

**Next:** Back Home →

</div>

</div>

</div>

<div>

<div>
