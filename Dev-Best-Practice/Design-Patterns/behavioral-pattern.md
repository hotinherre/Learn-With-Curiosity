# Behavioral patterns

manage communication between different class/entity.

## Chain of responsibility

there are more than one object may handle a request. encapsulate these handler object in a pipeline, then launch and leave request at the entrance of the pipeline.

## Command

encapsulate a request into an object for logging, queue or undo operation

## Interpreter

interpreting a language.

## Iterator

provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

## Mediator

Define an object to encapsulate N-M interaction of a set of objects to enable loose coupling.

# Memento

capture object internal state so that the object can be restored to this state later.

# Null Object

Avoid explicitly null pointer checking by provide an Null Object which do no-ops.

# Observer

pub-sub model. notify all dependents when object changes.

# State

allow object to change behaviour when state changes. encapsulate behaviour and state in same object

# Strategy

Provide a simple interface for client to select algorithm, bury implementation detail in derived class.

# Tempalte Method

Define the skeleton of the algorithm and allow subclass or client to redefine certian steps of the algorithm. Think sort in go.
