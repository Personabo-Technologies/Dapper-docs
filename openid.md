
# **Request Social Information from the User**

<div>

<div>

ℹ️

</div>

<div>

If you're writing an app with social features, such as a user profile,
or your app needs to contact them via email, you might want to obtain a
user's basic profile information. FCL's OpenID service on Dapper allows
your app to do that, while verifying the identity of a Dapper Wallet
user.

</div>

</div>

<div>

Your app requests information about the user during authentication with
the wallet. If the user consents to share this information, a signed and
encrypted JWT containing this information is returned to your
application. The JWT can be decrypted and verified by your application's
backend.

</div>

<div>

To request this information, you need to do two things:

</div>

1.  Configure the OpenID add-on for your application in the Dapper
    Developer Dashboard
2.  Configure FCL in your application to request OpenID information from
    the user during authentication to the wallet

# Configuring the OpenID add-on in the Dapper Developer Dashboard

## OpenID Configuration in the Dapper Developer Dashboard

<div>

OpenID parameters are configured through the Dapper Developer Dashboard,
in the `OpenID integration` section of each application page.

</div>

<div>

</div>

<div>

All parameters are required in order to use the OpenID functionality.
Click on `Save changes` upon the configuration of each field.

</div>

### Change Request for Live Applications

<div>

<div>

❗

</div>

<div>

An application in the `LIVE` state disables editing of most fields. To
update OpenID-related fields, please contact your Dapper customer
success representative with the corresponding values to be reviewed and
applied.

</div>

</div>

<div>

</div>

## OpenID Add-on Parameters

<div>

The `OpenID integration` add-on feature requires the following
parameters to be set:

</div>

## Key Generation

<div>

<div>

💡

</div>

<div>

We recommend using `openssl` to generate the keys. If you're on a Mac
with an M1 chip, use `Homebrew` to install it, as there have been some
issues with using the `openssl`version that comes preinstalled.

</div>

</div>

<div>

To generate the keys, run the following commands and store the private
key safely. You will need the private key to decrypt the JWT containing
the user's profile information.

</div>

<div>

    # generate RSA keys
    openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out dapp.key
    openssl rsa -in dapp.key -RSAPublicKey_out -pubout > dapp.pub

    # generate ECDSA keys
    openssl ecparam -name secp256k1 -genkey -noout -out dapp.key
    openssl ec -in dapp.key -pubout > dapp.pub

</div>

# Configuring FCL

<div>

FCL must be configured to request OpenID scopes (i.e. user profile
information) during authentication with the wallet.

</div>

<div>

Two aspects of FCL need to be configured:

</div>

1.  the `account-proof-resolver`, which is used to send the App
    identifier (set in the Dapper Developer Dashboard) to Dapper Wallet
2.  `openid-scopes`, which is the specific user information your
    application is requesting from the Dapper Wallet user

## account-proof-resolver

<div>

For the App identifier to be sent to Dapper Wallet during
authentication, an `account-proof-resolver` must be configured in FCL.
The configuration specifics will depend upon your application's specific
implementation, but the process is the same: configure FCL with an
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

    type AccountProofData {
      // e.g. "Awesome App (v0.0)" - A human readable string to identify your application during signing
      appIdentifier: string;  

      // e.g. "75f8587e5bd5f9dcc9909d0dae1f0ac5814458b2ae129620502cb936fde7120a" - minimum 32-byte random nonce as hex string
      nonce: string;          
    }

    type AccountProofDataResolver = () => AccountProofData

    const accountProofResolver: AccountProofDataResolver = () => ({
      appIdentifier: 'MyAwesomeApp',
      nonce: '0477edbb03e51892dbcdba2ce6c385c6804b423d0402c2a5891490d5c10c7f19'
    })

    config({
      "fcl.accountProof.resolver": accountProofResolver
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
`window.open`is greater than a certain threshold (exact number unknown,
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
      return await API('/account-proof-data');
    };

</div>

# openid-scopes

<div>

To request user information during authentication with the wallet,
configure the `service.OpenID.scopes` value in FCL with the desired
scopes. Currently, Dapper Wallet supports `email` and
`preferred_username` scopes:

</div>

<div>

    import { config } from '@onflow/fcl';

    config({
        'service.OpenID.scopes': 'email preferred_username',
    }); 

</div>

<div>

If the user consents to share this information, an `open-id` FCL service
will be returned as part of the authentication response.

</div>

<div>

<div>

💡

</div>

<div>

Dapper Wallet allows users to consent to individual scopes, so the
encrypted JWT may only contain a subset of the request scopes.

</div>

</div>

<div>

The `open-id` service has a `data` property that contains an encrypted
JWT with the user information that can be sent to your application's
backend for decryption and verification.

</div>

<div>

Your frontend application code can subscribe to updates to FCL's
`currentUser` object --- which fires once the user authenticates with
FCL --- to extract the encrypted JWT and send it to your application's
backend to decrypt and verify:

</div>

<div>

    type OpenIdService = {
      type: 'open-id',
      method: 'data',
      data: {
        token: string,
      }
    }

    useEffect(() => {
        return currentUser.subscribe(async (user) => {
          const openIDService: OpenIdService = user.services?.find(
            (service) => service.type === 'open-id',
          );

          if (openIDService) {
            // send encrypted JWT to backend for verification
            const response = await fetch('/api/open-id', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
              },
              body: JSON.stringify(openIDService.data),
            });

            const data = await response.json();

            setOpenIDData(data);  
          } else {
            setOpenIDData(null);
            }
        });
      }, []);

</div>

<div>

Your application's backend will receive an encrypted JWT with a
******nested****** signed JWT containing the user information. The
backend should first decrypt the JWT to extract the nested signed JWT,
then verify the signature on the nested JWT. If both operations are
successful, the user information can be used:

</div>

<div>

    import { NextApiRequest, NextApiResponse } from 'next';
    import { JWK, JWE, JWS } from 'node-jose';

    async function decrypt(token: string) {
      const privateKey = process.env['OPEN_ID_PRIVATE_KEY'];

      const keystore = JWK.createKeyStore();
      await keystore.add(await JWK.asKey(privateKey!, 'pem'));

      const decryptedVal = await JWE.createDecrypt(keystore).decrypt(token);

      const claims = Buffer.from(
        (decryptedVal as JWE.DecryptResult).plaintext,
      ).toString();

      return claims;
    }

    async function verify(claims: string) {
      let payload = {};

      try {
        const result = await JWS.createVerify().verify(claims, {
          allowEmbeddedKey: true,
        });
        payload = Buffer.from(result.payload).toString();
      } catch (err) {
        console.log(err);
      }

      return payload;
    }

    export default async function openid(
      req: NextApiRequest,
      res: NextApiResponse,
    ) {
      try {
        const { token } = req.body;
        const jwtToken = await decrypt(token);
        const payload = await verify(jwtToken);

        res.status(200).json(payload);
      } catch (err: any) {
        console.log(err);
        res.status(400).json({ error: err.message });
      }
    }

</div>

# FAQ

<div>

<div>

❓

</div>

<div>

**Why is the application not getting any FCL OpenID service back from
the FCL Auth-N request?** Please check the following scenarios. 1.
OpenID parameters are not populated in the Dapper Developer Dashboard.
2. The FCL App Id and/or the Account Proof Identifier mismatches the App
Identifier configuration in the Dapper Developer Dashboard.

</div>

</div>

<div>

<div>

❓

</div>

<div>

**How come in the Dapper Developer Dashboard, the fields are locked and
OpenID cannot be configured?** The application is in the `LIVE` state.
Please submit a request with the configurations to your Dapper customer
support representative.

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
