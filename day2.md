# Refresher

Ideas from day1.

* Event Storming
* Event Modeling (the blueprint)
* Command Query Responsibility Segregation with Event Sourcing (CQRS/ES)

# Common Antipaterns 

M.E.S.S. Monolithic Event Sourced System.
* Everything is seeing your events, so the event store become the canonical model (M.E.S.S.)
* Public final state :-(

E.S.S. Event Sourced State
* Private state (internal to the service) derives itself from a private stream of facts (events).
* Public state (the public schema) is the output of a translator that turns private state into public messages.
* At any time, we can rebuild both private and public state from the private stream of facts.

> Aside: Events are in the domain model; commands are not in the domain model.

https://github.com/condron?tab=repositories
* CQRS-Training
    * Account Balance
    * Restaurant
    * Stop Loss
