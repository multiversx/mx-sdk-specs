## PublicKeyEncryptedData

Optional, can be omitted by implementing libraries.

Implementation suggestions:
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/crypto/x25519EncryptedData.ts

```
dto PublicKeyEncryptedData:
    nonce: string;
    version: number;
    cipher: string;
    ciphertext: string;
    mac: string;
    identities: PublicKeyEncryptedDataIdentities;

dto PublicKeyEncryptedDataIdentities:
    recipient: string;
    ephemeralPubKey: string;
    originatorPubKey: string;
```
