---
layout: documentation
doctitle: Features
---

[Edit document on Github](https://github.com/sirixdb/sirixdb.github.io/edit/master/features.md)

### Features in a nutshell
- Currently native XML and JSON storage (other data types might follow).
- No write-ahead log needed, always consistent on the flash drive as the UberPage is the main entry point to the storage and written last to a new location.
- No background compaction process or gradual merging/compaction is needed as in LSM-trees.
- Natural multi-version concurrency control (MVCC), each read-only transaction operates on one revision/snapshot, only one read/write transaction can co-exist. We only need a lock for the single writer (in the future we might apply optimistic concurrency control On the database level)
- Rollback to a past revision of a document/resource is supported, which encourages doing experiments or to solve human- or application errors.
- Transactional, versioned, typed user-defined index-structures, which are automatically updated once a transaction commits.
- Through XPath-axis extensions we support the navigation not only in space but also in time (future::, past::, first::, last::…). Furthermore, we provide several temporal XQuery functions due to our integral versioning approach. Temporal navigation for JSON resources is done via built-in XQuery functions.
- An in-memory path summary, which is persisted during a transaction commit and always kept up-to-date.
- Configurable versioning at the database level (full, incremental, differential, and a novel sliding snapshot algorithm that balances read and writes without introducing write-peaks, which are usually generated during intermediate full dumps, which are usually written to).
- Log-structured sequential writes and random reads due to transactional copy-on-write (COW) semantics. This offers nice benefits as no locking for concurrent reading transactions and it takes full advantage of flash disks while avoiding their weaknesses.
- Complete isolation of currently N read-transactions and a single write-transaction per resource.
- The page structure is heavily inspired by ZFS and therefore also forms a tree. We’ll implement a similar merkle-tree and store hashes of each page in parent-pointers for integrity checks.
- Support of XQuery and XQuery Update due to a slightly modified version of brackit(.org).
- Moves for the XML layer are additionally supported.
- Automatic path-rewriting of descendant-axis to child-axis if appropriate.
- Import of differences between two XML-documents, that is after the first version of an XML-document is imported an algorithm tries to update the Sirix resource with a minimum of operations to change the first version into the new version.
- A fast ID-based diff-algorithm which can determine differences between any two versions of a resource stored in Sirix optionally taking hashes of a node into account.
- The number of children of a node, the number of descendants, a hash as well as an ORDPATH / DeweyID label which is compressed on disk to efficiently determine document order as well as to support other nice properties of hierarchical node labels is optionally stored with each node. Currently, the number of children is always stored and the number of descendants is stored if hashing is enabled.
- Flexible backend.
- Optional encryption and/or compression of each page on disk.
