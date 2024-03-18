# Architecture of a DBMS: SQL Client

## Parsing and Optimization

- Parse, check, verify SQL
- Translate SQL into efficient relational query plan

## Relational Operators

- Execute dataflow by operating on records, files

## Files and Index Management

- Organize tables and records as groups of pages in logical file

## Buffer Management

- Provide illusion of operating in memory

## Disk Space Management

- Translate page requests into physical bytes on 1+ devices

## Overall

- Organized in layers
- Each layer abstracts layer below (manage complexity, performance assumptions)
- DBMS: 2 cross-cutting issues related to storage and memory management
  - Concurrency control
  - Recovery

## Disks

- Most database systems originally designed for magnetic disks -> implications for solid state disks too!
  - No pointer derefs. Instead API for READ and WRITE. Both are very slow.
    - Transfering data from disk to RAM (and vice versa)
    - Explicit API can help minimize pointer errors

## Storage Hierarchy

in order of big, slow to small, fast:

- Disk -> SSD -> RAM -> L2 Cache -> L1 Cache -> Registers

## Components of a Disk

- Platters spin ~15000 RPM
- Arm assembly moves to position head on desired track
- **only 1 head reads or writes at a time**
- time to access (read/write) a disk block:
  - seek time: moving arms to position disk head on track
  - rotational delay: waiting for block to rotate under head
  - transfer time: moving data to/from disk service
- to lower I/O cost: minimize seek and rotational delays

## Flash (SSD)

- **Reading** fast + predictable, unlike writing
  - Fine-grain reads (4-8K reads), coarse-grain writes (1-2 MB writes)
- Only 2k-3k erasures before failure
- Write amplification: big units, reorg for wear + garbage collection
- Flash reads much faster than Disk, can be 10-100x bandwidth compared to HHD
