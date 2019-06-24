
# Event Modeling

## Adam's Thesis

* Digital information systems are nothing special.
* All we have done is to digitize existing, brick-and-mortar processes.
* During digitization, thought, we changed existing processes to meet the constraint of expensive digital storage.
* Changing the long-standing, historical, processes made systems hard for laypersons to understand.
* The accidental complexity of meeting that constraint added unnecessary complexity.
* Now that digital storage is less expensive, we can remove that accidental complexity.
* Now, our digital systems can better reflect historical brick-and-mortal processes.
* This makes the system less complex and easier for business stakeholders to comprehend. 

## Example 

Commands 
* AddTodo
* FinishTodo

Commands
* AddedTodo
* FinishedTodo

ReadModel / Projection
* List of all todo items

[Would a Todo item be considered an aggregate?]

## Costing / Project Management

* Cost per feature becomes linear because there is less communication among parts.
* Cost is per command/event pair.
* Velocity remains constants throughout the project.
* Velocity measurement is simpler as time per command/event pair.

> Fish shell (friendly interactive shell) comes highly recommended (https://fishshell.com)

## Specification Style for Creating the Blueprint

Command language among developers and business stakeholders.

Everyone who cares or might care is in the room during the generation of the blueprint.

The blueprint is a timeline that moves from left to right.

Use an online whiteboard to store this digitally.
E.g. Miro

Write Model

* User Interface Wirefame
* Given: Command with all data fields
* When: Command validation
* Then: Event with all data fields / Failure with error message

Read Model

* User Interface Wirefame
* Given: Query
* Then: Data
* There is no `when` in this situation, because there is no validation necessary.

Actions/Users/Interacting Systems > Use Swim Lanes for this.

Translation

* External System Messages (e.g. GPS coordinations) -----> Domain Events (entered downtown).
* Domain Events -----> External System Messages.

If it is not on the final blueprint, then we are not going to build it.
Make sure that the whole system touches, via a thread, through the blueprint (the long sticky note white board).
The blueprint one simple diagram that contains everything the system needs.

Use common sense to determine when to start a new blueprint instead of including multiple systems in one massive blueprint.
* As a guideline, use Conways Law as a heuristic.
* In large companies it is reasonable to break multiple systems into multiple blueprints instead of one mono-blueprint.
