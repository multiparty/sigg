# SIGG
Standards and conventions governing data structures and message formats that may be utilized by frameworks and libraries that implemenat garbled circuit protocols.

## Purpose
These standards and conventions govern the design and implementation of garbled circuit protocol libraries. They serve at least two purposes. First, they establish common formats for messages and data that must be exchanged by protocol participants. If different implementations (potentially built using different frameworks and targeting different platforms) adhere to these conventions, they should be mutually compatible and interoperable. Second, they articulate conventions for data structures used in such libraries. These conventions can guide design decisions at the time of implementation, thus helping reduce the number of varations across different implementations (when such variations serve no other legitimate engineering or design purpose).
