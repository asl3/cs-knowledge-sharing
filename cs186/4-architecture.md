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
    - Explicit API can help minimize pointer errors
