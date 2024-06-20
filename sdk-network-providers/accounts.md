class AccountOnNetwork:
    address: IAddress
    nonce: int
    balance: Amount
    code: bytes
    username: str
    codeHash: str

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]) -> AccountOnNetwork;

    // optionally, a method to convert the object to a dictionary/plain object can be implemented
    to_dictionary() -> Dict[str, Any];


class Guardian:
    activationEpoch: int
    address: IAddress
    serviceUid: str

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]) -> Guardian;


class GuardianData:
    guarded: boolean
    activeGuardian: Guardian
    pendingGuardian: Guardian

    / The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be left empty; a static method can be used for initialization
    static from_http_response(payload: Dict[str, Any]): GuardianData;

    get_current_guardian_address(): None | IAddress;
