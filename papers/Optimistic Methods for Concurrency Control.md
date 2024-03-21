# On Optimistic Methods for Concurrency Control

H.T. KUNG and JOHN T. ROBINSON
Carnegie-Mellon University

Notes by @asl3

- Transaction: sequence of accesses to database that preserves integrity constraints of database
- Concurrency enables efficient use of hardware
- Multiple processors
- So much data that primary memory can only hold fraction of database, and data must be swapped in from secondary memory
- Locks maintain integrity of database! But locks have disadvantages
- Overhead for lock maintenance, deadlock detection
- No general-purpose protocols that always provide high concurrency
- Significantly lowers concurrency
- If congested node (frequently accessed node, like root of tree) and much data in secondary memory -> waiting for secondary memory access
- Locks cannot be released until end of transaction, to allow transaction to abort if mistakes
- Most important: locking may only be necessary in the worst case!
