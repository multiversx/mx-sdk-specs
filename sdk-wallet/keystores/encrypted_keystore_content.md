## EncryptedKeystoreContent

Also see (mere references, not implementation suggestions):
 - `encryptedKeyJSONV4`: https://github.com/multiversx/mx-sdk-go/blob/v1.3.8/interactors/wallet.go
 - Transforming the keystore content into an `PasswordEncryptedData`: https://github.com/multiversx/mx-sdk-js-wallet/blob/v4.2.1/src/userWallet.ts#L127

`EncryptedKeystoreContent` DTO is a representation of the JSON keystore file. 

Generally speaking, decrypting the payload of a keystore file happens as follows (high-level):
 - the content of the keystore file is loaded into a `EncryptedKeystoreContent`.
 - the `EncryptedKeystoreContent` is mapped (converted) to a `PasswordEncryptedData`
 - the `PasswordEncryptedData` is fed to an `IPasswordBasedDecryptor` (e.g. `PasswordBasedEncryptorDecryptor`) for decryption.
 - the decrypted payload is interpreted downstream (e.g. as a secret key, mnemonic etc.).


Generally speaking, saving a confidential payload into a keystore file happens as follows (high-level):
 - the payload is encrypted using an `IPasswordBasedEncryptor` (e.g. `PasswordBasedEncryptorDecryptor`). What results is a `PasswordEncryptedData`.
 - the resulted `PasswordEncryptedData` is mapped (converted) to an `EncryptedKeystoreContent`.
 - the `EncryptedKeystoreContent` is written to a file.

```
dto EncryptedKeystoreContent:
    version: number;

    // "secretKey|mnemonic"
    kind: string;
    
    // a GUID
    id: string

    // hex representation of the address
    address: string 

    // bech32 representation of the address
    bech32: string

    crypto: {
        // PasswordEncryptedData.ciphertext
        ciphertext: string;

        cipherparams: {
            // PasswordEncryptedData.iv
            iv: string;
        };

        // PasswordEncryptedData.cipher
        cipher: string;

        // PasswordEncryptedData.kdf
        kdf: string;
        kdfparams: {
            // PasswordEncryptedData.kdfparams.n
            n: number;

            // PasswordEncryptedData.kdfparams.r
            r: number;

            // PasswordEncryptedData.kdfparams.p
            p: number;

            // PasswordEncryptedData.kdfparams.dklen
            dklen: number;
            
            // PasswordEncryptedData.salt
            salt: string; 
        };

        // PasswordEncryptedData.mac
        mac: string;
    };
```
