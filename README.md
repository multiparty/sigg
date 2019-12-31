# SIGG
Standards and conventions governing data structures and message formats that may be utilized by frameworks and libraries that implemenat garbled circuit protocols.

## Purpose
These standards and conventions govern the design and implementation of garbled circuit protocol libraries. They serve at least two purposes. First, they establish common formats for messages and data that must be exchanged by protocol participants. If different implementations (potentially built using different frameworks and targeting different platforms) adhere to these conventions, they should be mutually compatible and interoperable. Second, they articulate conventions for data structures used in such libraries. These conventions can guide design decisions at the time of implementation, thus helping reduce the number of variations across different implementations (when such variations serve no other legitimate engineering or design purpose).

## Schemas
A number of JSON schemas for common data structures and message formats are included in this repository. These schemas are inspired by the established [Bristol Fashion](https://homes.esat.kuleuven.be/~nsmart/MPC/) format, which is supported by several compiled MPC libraries such as [SCALE-MAMBA](https://homes.esat.kuleuven.be/~nsmart/SCALE/). A few optional extensions have been introduced into the data structure definitions that may be useful within implementations built using certain programming languages.

### Unmodified Circuits and Gates
The following JSON schemas for data structures typically used to represent data at rest or within a running process are included under [`schemas/`](schemas/):
* `circuit.schema.json`: Representation for a circuit that has the same information found in a Bristol Fashion circuit specification, plus some optional fields that may be helpful for certain implementations
* `gate.schema.json`: Representation for an individual gate (identical to the definition referenced in `circuit.schema.json`)
* `gates.schema.json`: Representation for an an indexed collection of gates (isomorphic to the definition referenced in `circuit.schema.json`)

### Modified Circuits and Gates
The following JSON schemas for data structures typically used to represent modified data structures or to transfer data between protocol participats are also included under [`schemas/`](schemas/):
* `label.schema.json`: Representation of a label (i.e., to be applied to circuit/gate) as an array of 8-bit unsigned integers (i.e., [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array)
* `gates.garbled.schema.json`: Representation for an indexed collection of gates in their garbled representation, where:
  * each garbled gate is represented either in as an array of four labels (i.e., the original gate was encrypted)
  or as an empty array (i.e., the original gate was not encryptes
  * the information included must be used in conjunction with the original collection of gates from a circuit to determine, for example, the original operation for each gate
* `assignment.schema.json`: Representation for a map from wire indices to arrays of labels (e.g., to be used to represent the labels assigned to each wire in a circuit), where:
  * the indices are drawn from with the original circuit definition
