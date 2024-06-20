interface IContractQuery{
    address: IAddress
    caller: Optional[IAddress]
    function: string
    value: string

    get_encoded_arguments(): List[string];
}

interface IPagination {
    from: number;
    size: number;
}