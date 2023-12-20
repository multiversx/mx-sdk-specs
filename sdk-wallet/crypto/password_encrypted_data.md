## PasswordEncryptedData

Implementation suggestions:
  - `EncryptedData`: https://github.com/multiversx/mx-sdk-js-wallet/blob/v4.2.1/src/crypto/encryptedData.ts
  - `EncryptedData`: https://github.com/multiversx/mx-sdk-py-wallet/blob/v0.8.3/multiversx_sdk_wallet/crypto/encrypted_data.py

**Note:** this DTO is not a representation of the JSON keystore itself. Instead, it defines a data structure that is **agnostic to the type of file it might be stored in**. This abstraction was introduced July 2021:
 - https://github.com/multiversx/mx-sdk-js-core/pull/11

In practice, such a data structure is, indeed, stored in a JSON keystore file, whose schema does resemble the DTO below - but it's not identical. Any similarities between `PasswordEncryptedData` and `EncryptedKeystoreContent` **should not be considered duplication**.

```
dto PasswordEncryptedData:
    id: string;
    version: number;
    cipher: string;
    ciphertext: string;
    iv: string;
    kdf: string;
    kdfparams: object;
    salt: string;
    mac: string;

// Example of class compatible with "PasswordEncryptedData.kdfparams".
// Can be anything, and it's correlated to a "PasswordEncryptedData.kdf" (e.g. "scrypt").
// EncryptorDecryptor.encrypt() puts this into "PasswordEncryptedData.kdfparams".
// EncryptorDecryptor.decrypt() interprets (possibly validates) it.
dto ScryptKeyDerivationParams:
    // numIterations
    n = 4096;

    // memFactor
    r = 8;

    // pFactor
    p = 1;

    dklen = 32;
```
