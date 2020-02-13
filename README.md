# SIGG
Standards and conventions governing data structures and message formats that may be utilized by frameworks and libraries that implemenat garbled circuit protocols.

## Purpose
These standards and conventions govern the design and implementation of garbled circuit protocol libraries. They serve at least two purposes. First, they establish common formats for messages and data that must be exchanged by protocol participants. If different implementations (potentially built using different frameworks and targeting different platforms) adhere to these conventions, they should be mutually compatible and interoperable. Second, they articulate conventions for data structures used in such libraries. These conventions can guide design decisions at the time of implementation, thus helping reduce the number of variations across different implementations (when such variations serve no other legitimate engineering or design purpose).

## Structured Representations
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

## Serialized Representations

### Serialized Representation of Garbled Gates (SRGG)

The SRGG format represents an ordered collection of wire label sequences as a byte stream that is amenable to relatively straightforward implementations in different languages and platforms. This format assumes that the number of bits in an individual label is a multiple of eight. The bytes are ordered in the following way:

* The first byte represents the number of bytes in each label (which we call `b`)
* The next four bytes represent the total number of entries (which we call `n`)
  * The first byte is the 8 least significant bits (i.e., `n % 256`)
  * Subsequent bytes are in increasing order of significance
* All subsequent bytes represent each of the `n` individual entries (i.e., individual label sequences containing zero or more labels)
  * The first byte (and possibly only byte) of each entry represents the operation:
    * A value of `0` indicates that there is no operation and that the label sequence has no length and no entries
    * A value of `1` represents `AND`
    * A value of `2` represents `XOR`
    * A value of `3` represents `NOT`
  * If the first byte is not `0`, then the second byte of each entry is the number of labels (which we call `k`) in the sequence corresponding to that entry
  * If the number of labels in the sequence is greater than zero, then the next `k * b` bytes represent the label data, with each string of `b` bytes representing one of the labels
