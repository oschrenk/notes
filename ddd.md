# Domain Driven Design
/Domain Event/ is a record of some business-significant occurrence in a /Bounded-Context/

## Tactical Design
* having a `occuredOn: DateTime` is probably the minimum interface on events
* each domain event should be statement of a past occurence - a verb in past tense eg `SprintScheduled`
* normally these events are the result of a /Command/ eg `ScheduleSprint`.
* the properties of a `SprintScheduled` event should hold all the properties of the command that created it eg `sprintId`, `productId`, `name`, `startDate`, and `endDate`
* be careful not to fill event with more properties
* important to modify /Aggregate/ and Domain Event as a transaction
* storing Domain Events in its causal order does not guarentee that it will arrive in the same order
* consuming Bounded Context

## Glossary
*Aggregate*

*Domain Event*

*Event Sourcing*
Persisting all Domain Events that have occurred for an Aggregate.

*Bounded-Context*

*Ubiquotuous Language*
