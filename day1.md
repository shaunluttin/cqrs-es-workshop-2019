## Key Terms

Event Storming. Generate the ideas.
Event Modeling. Create the blueprint.
Event Sourcing. The change log becomes the primary source of data.
Event Store. A stream database.

## Value Proposition

Audits. 
* With event sourcing the current app state is has a provable auditlog.
* With event sourcing we can tell when customers do things (e.g. order on what date).

Distrubuted Systems.
* Local logs of events capture work completed.
* Those local logs then publish.

Developer Velocity
* Removal of accidental complexity because code is aligned with business language.

More Natural Human Implementation Information System (Adam)
* Additional computer memory means we can now keep a ledger.
* These ledgers have been how humans organize for years (medicine, accounting)
* People like a storyline, "how did we get here?"
* Event Sourcing gives us a narrative, a history.
* How we think as humans and Moores Law.

Lossless
* No loss of information.
* Always able to restore transient states.

Modeling
* Replay history and change things.
* We now have a history that we can use as a model.

Rebuilding
* We can rebuild the system state to any time.
* This also helps to restore after failures.

## Fundamentals

Commands come into the system.
The appropriate BLL/Domain service approves the command.
* approval checks the invarients.
* invarient is a synonym for a business rule.
The command writes an event to its stream.

Concerns
* If perf. ever becomes a problem then hydrate from the last snapshot.
* If the devs are smart enough to write snapshots, then they are smart enough to hydrate in real-time.

Caching & Perf
* Caching: Event store snapshots can be stale but not invalid. This solves cache invalidation.
* For performance, set a Service Level Aggreement (SLA) that governs when to create a snapshot (if at all). 
* We can have a rule for perf such as... if stream lengths exceeds 1000 events, then write a snapshot.

Business Rules / Invarients / Stop the World
* These live in the aggregate.
* The aggregate is what validates/approves the command.

Aggregates in CQRS/ES <----------------------------------------------------
* [Is this a command?]
* [Does the aggregate do command validation?]
* [What about indexing and query keys?]
* [Where does the aggregateId come into play?]
* [Could we please see an aggregate or build one together.]
* [Do we store the event streams in separate tables or do we identify them via aggregateId/index?]

Manage Excessive Complexity
* Factorials are a great proxy for the complexity of systems.
* [This reminds me of the mythical man month, in which complexity grows.]
* Failure to manage this "accidental" complexity is what kills most software systems.
* The most productive developers write the least amount of code.

Write Side
* Command
* Handler
* Aggregate Hydration (3rd normal form - hydrate only what is necessary for validation)
* Invariant Validation
* Write to the aggregate specific stream
* General rule/summary: one controller on top of one stream.

Read Side
* Polyglot
* Bespoke (custom made - specifically for the view)
* 1st normal form

Words Terms that I might have missed:
* vector clock. this is for ordering (better than timestamp) for use with multiple streams (streamId + incrementing index).
* HA vs DR (disaster recovery)

> Aside: Always use GUIDs for IDs instead of natural ID (or in addition to natural IDs).

### Building Write-Side Services

Hexagonal Architecture / Ports & Adapters Architecture
* Business Logic in the core.
* Services surround the Business Logic.
* Client talks to services.
* Database talks to services.

Command is a data transfer object (DTO) aka property bag
Command enters a service which takes three steps:
1. hydrate
    * minimally populate the aggregate
    * draw the data from the event source
    * if we have yet to call `apply()`, how do we know what data to load?
2. apply [is this also 'validate/approve']
    * call one or more methods on the aggregate
    * if those methods succeed, then the command is approved
3. save
    * call save on the aggregate
    * the aggregate writes to its event stream
    * [this implies that the aggregate is data-store aware... is that true? does the aggregate publish an event?]
Private business events.

// There is not a one to one mapping betweem commands and events.
class Order {

    private _otherVariablesNecessaryForValidation;

    private pendingEvents: Event[] = [];

    /* Populate variables necessary for validation. */
    public hydrate(events: Event[]);

    /* Perform validation and raise the event if valid. */
    public placeOrder(quantity: number);

    /* The repository calls this on save. */
    public getAndClearAllPendingEvents();

    /* Apply the event to the class instance local state. */
    private apply(event: Event);

    /* Add validated event to pendingEvents and then call apply(). */
    private raise(event: Event);
}

Don't return any state on the command; only return pass/fail, when fail include the exception message.

### Next Example

`=>` is a boundary between layers.
`->` is with a layer.

App -> cmd 
    => service -> domain with 'private events' -> service 
        => event store with 'private events' 
            => translator -> 'public messages' 
                => external services

Private Events
* targeted
* business oriented
* sparse
* fine grained

Public Events (aka Messages)
* complete (as opposed to sparse)
* the public events aggregate together related events 
* ...so that external services do not need to callback for more info.
* They are fundamentally a public read model that we publish to external services.
* This is the external API contract that allows us to be dynamic with our private domain events without breaking the contract.

Event Source Architecture (private)
Event Driven Architecture (public)

### Testing

Test at the level of analysis of input commands and output events.

These tests do not need to reference aggregates.

### Personal Overview right Now

Client Application(s)
     |
  Commands (DTO)
     |
Input Services (Handlers)
     |
Domain
* Aggregates
* Invarients (business rules)
* Private Events
     |
  Events (DTO)
     |
Output Services (Repositories)
     |
Event Store
     |
Read Models
