## KeyPairBasedEncryptorDecryptor

Optional, can be omitted by implementing libraries.

Implementation suggestions:
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/v4.2.1/src/crypto/pubkeyEncryptor.ts
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/main/src/crypto/pubkeyDecryptor.ts
 - 

```
class KeyPairBasedEncryptorDecryptor:
    encrypt(data: bytes, recipient_public_key: IPublicKey, auth_secret_key: ISecretKey): PublicKeyEncryptedData;

    decrypt(data: PublicKeyEncryptedData, decryptor_secret_key: ISecretKey): bytes;
```
