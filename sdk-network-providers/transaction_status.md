class TransactionStatus:
    status: string

    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // can be something like:
    constructor(status: string | None);

    is_pending(): boolean;

    is_successful(): boolean;

    is_invalid(): boolean;

    is_failed(): boolean;

    is_executed(): boolean;

    equals(other: TransactionStatus): boolean;

    // should also implement a method to return the status as a string
    to_string(): string;