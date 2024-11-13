## TransactionEventsParser

```
class TransactionEventsParser:
    // The constructor is not captured by the specs; it's up to the implementing library to define it.
    // Generally speaking, the constructor can be parametrized with an `Abi` or `Codec` instance (implementation detail), necessary to decode contract return data.

    // Parses a list of events, leveraging the ABI. The ABI definition is picked based on the event's identifier.
    parse_events(events: List[TransactionEvent]): List[any];

    // Parses an individual event, leveraging the ABI. The ABI definition is picked based on the event's identifier.
    parse_event(event: TransactionEvent): any;
```
