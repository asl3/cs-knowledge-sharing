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

## Storage Pragmatics

- Many significant databases are not large
  - BUT, data sizes grow faster than Moore's Law

## Hardware Bottom Line

- Very large DBs: disk is best cost/MB, SSDs improve performance and performance variance
- Smaller DBs: hardware changing quickly, flash wins at low end

## Block Level Storage

- Read and write large chunks of sequential bytes
- Amortize seek delays (HDDs) and writes (SSDs)
- Predict future behavior
  - Cache popular blocks
  - Pre-fetch blocks likely to be accessed
  - Buffer writes to sequential blocks

## Terminology Notes

- Block = unit of transfer for disk read/write
- Page = common synonym for block
- 'Next' block concept
  - sequential blocks on same track
    - followed by blocks on same cylinder
    - followed by blocks on adjacent cylinder
- Sequential scan: pre-fetch several blocks at any time!
- Read large consecutive blocks

## Disk Space Management

- Lowest layer of DBMS, manages space on disk
- Purpose:

  - Map pages to locations on disk
  - Load pages from disk to memory
  - Save pages back to disk, ensure writes

- Requesting pages
  - request for sequence of pages best satisfied by pages stored sequentially on disk
  - physical details hidden from higher levels of system
  - higher levels may safely assume next page is fast

## Disk Space Management: Implementation

- Proposal 1: talk to storage device directly

  - fast, but what happens when devices change?

- Proposal 2: Run over filesystem (FS)

  - allocate single large contiguous file on empty disk, assume sequential/nearby byte access are fast
  - most FS optimize disk layout for sequential access
  - DBMS file can span multiple FS files on multiple disks/machines!!

- Summary: Disk Space Management
  - Provide API to read + write pages to device
  - Pages: block level organization of bytes on disk
  - Provides "next" locality and abstracts FS/device details
