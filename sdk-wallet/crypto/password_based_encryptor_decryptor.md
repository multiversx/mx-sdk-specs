## PasswordBasedEncryptorDecryptor

Implementation suggestions:
 - https://github.com/multiversx/mx-sdk-py-wallet/blob/v0.8.3/multiversx_sdk_wallet/crypto/encryptor.py
 - https://github.com/multiversx/mx-sdk-py-wallet/blob/v0.8.3/multiversx_sdk_wallet/crypto/decryptor.py
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/v4.2.1/src/crypto/encryptor.ts
 - https://github.com/multiversx/mx-sdk-js-wallet/blob/v4.2.1/src/crypto/decryptor.ts

```
class PasswordBasedEncryptorDecryptor:
    encrypt(data: bytes, password: string): PasswordEncryptedData;

    decrypt(data: PasswordEncryptedData, password: string): bytes;
```
