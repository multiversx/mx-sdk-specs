## TokenManagementTransactionsFactory

A class that provides methods for creating transactions for token management operations.

```
class TokenManagementTransactionsFactory:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor should be parametrized with a configuration object which defines entries such as:
    // "minGasLimit", "gasLimitPerByte", "issueCost", gas limit for specific operations etc. (e.g. "gasLimitForSettingSpecialRole").

    create_transaction_for_issuing_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        initialSupply: int;
        numDecimals: int;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;
    }): Transaction;

    create_transaction_for_issuing_semi_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;
    }): Transaction;

    create_transaction_for_issuing_non_fungible({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;
    }): Transaction;

    create_transaction_for_registering_meta_esdt({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        numDecimals: number;
        canFreeze: boolean;
        canWipe: boolean;
        canPause: boolean;
        canTransferNFTCreateRole: boolean;
        canChangeOwner: boolean;
        canUpgrade: boolean;
        canAddSpecialRoles: boolean;
    }): Transaction;

    create_transaction_for_registering_and_setting_roles({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: RegisterAndSetAllRolesTokenType;
        numDecimals: number;
    }): Transaction;

    create_transaction_for_setting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_unsetting_burn_role_globally({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_setting_special_role_on_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleLocalMint: boolean;
        addRoleLocalBurn: boolean;
        addRoleESDTTransferRole: boolean;
    }): Transaction;

    create_transaction_for_setting_special_role_on_semi_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleNFTCreate: boolean;
        addRoleNFTBurn: boolean;
        addRoleNFTAddQuantity: boolean;
        addRoleESDTTransferRole: boolean;
        addRoleESDTModifyCreator: boolean;
    }): Transaction;

    create_transaction_for_setting_special_role_on_non_fungible_token({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
        addRoleNftCreate: bool;
        addRoleNftBurn: bool;
        addRoleNftUpdate_attributes: bool;
        addRoleNftAddUri: bool;
        addRoleESDTTransferRole: bool;
        addRoleESDTModifyCreator: boolean;
        addRoleNFTRecreate: boolean;
    }): Transaction;

    create_transaction_for_creating_nft({
        sender: IAddress;
        tokenIdentifier: string;
        initialQuantity: int;
        name: string;
        royalties: int;
        hash: string;
        attributes: bytes;
        uris: List[string];
    }): Transaction;

    create_transaction_for_pausing({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_unpausing({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_freezing({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_unfreezing({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_wiping({
        sender: IAddress;
        user: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    create_transaction_for_local_minting({
        sender: IAddress;
        tokenIdentifier: string;
        supplyToMint: int;
    }): Transaction;

    create_transaction_for_local_burning({
        sender: IAddress;
        tokenIdentifier: string;
        supplyToBurn: int;
    }): Transaction;

    create_transaction_for_updating_attributes({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
        attributes: bytes;
    }): Transaction;

    create_transaction_for_adding_quantity({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
        quantityToAdd: int;
    }): Transaction;

    create_transaction_for_burning_quantity({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
        quantityToBurn: int;
    }): Transaction;

    // receiver is the same as the sender
    create_transaction_for_modifying_royalties({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
        new_royalties: int;
    }): Transaction;

    // receiver is the same as the sender
    create_transaction_for_setting_new_uris({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
        new_uris: List[string];
    }): Transaction;

    // receiver is the same as the sender
    create_transaction_for_modifying_creator({
        sender: IAddress;
        tokenIdentifier: string;
        tokenNonce: int;
    }): Transaction;

    // receiver is the same as the sender
    create_transaction_for_updating_metadata({
        sender: IAddress;
        tokenIdentifier: string;
        new_tokenName?: string;
        new_royalties?: int;
        new_hash?: string;
        new_attributes?: bytes;
        new_uris?: List[string];
    }): Transaction;

    // receiver is the same as the sender
    create_transaction_for_metadata_recreate({
        sender: IAddress;
        tokenIdentifier: string;
        tokenName?: string;
        royalties?: int;
        hash?: string;
        attributes?: bytes;
        new_uris?: List[string];
    }): Transaction;

    // receiver is the esdt manager address
    create_transaction_for_changing_token_to_dynamic({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    // receiver is the esdt manager address
    create_transaction_for_updating_token_id({
        sender: IAddress;
        tokenIdentifier: string;
    }): Transaction;

    // receiver is the esdt manager address
    create_transaction_for_registering_dynamic_token({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: "NFT" | "SFT" | "META" | "FNG";
    }): Transaction;

    // receiver is the esdt manager address
    create_transaction_for_registering_dynamic_and_setting_roles({
        sender: IAddress;
        tokenName: string;
        tokenTicker: string;
        tokenType: "NFT" | "SFT" | "META" | "FNG";
    }): Transaction;
```
