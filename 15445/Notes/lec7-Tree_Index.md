# Table Index
Replica of subset of table's attributes that are organized for efficient access.

## B tree
B+ tree is a self balanced tree that keeps data sorted and allows searches, sequentail access, insertions and deletions in O(log n).
- perfect balanced(same depth of node)
- every node other than root is half-full: `M/2 - 1 <= #keys <= M - 1`
- every inner node with k keys has k+1 non-null children

![node interior](assets/b-tree-leaf-node.png)

### Leaf node values
- Record IDs
  - a pointer to the location of tuple to which the index entry corresponds
- Tuple data
  - leaf node store actual contents of tuple
  - Second indexes must store the record id as values

### Insert
- Find correct leaf node L.
- Put data into L in sorted order.
  - If L has enough space, done!
  - Otherwise split L keys into L and new node L2
    - Redistribute entries evenly, copy up middle key
    - insert index entry to L2 into L

### Delete
- Find leaf L where entry belongs
- Remove entry
  - If L still half full, done!
  - Otherwise, redistribute from sibling
    - borrowing from slibing
    - if redistribution fails, merge L and sibling
    - if merge occured, delete entry from parent

### Duplicate keys
- Append Record ID: add tuple's unique Record ID as part of key to ensure uniqueness
- Overflow Leaf Nodes: Allow leaf to spill to overflow nodes that contain duplicate keys

### Clustered Indexes
Table is stored in the sort order specified by primary key.  
Traverse to the left-most leaf page and then retrieve tuples from all leaf pages.

### B-Tree Design Choice
- Node Size: slower storage device, larger node size.
- Merge Threshold: 
  - not always merge nodes when half full
  - delay a merge operation to reduce reorganization
  - leave small nodes and merge periodically
- Variable-length keys
  - Pointers - keys as pointers to tuple's attribute
  - Variable-length nodes - size of each node could vary
  - Padding - always padding key to be max length of key type
  - Key-map, indirection - Embed an array of pointers that map to the key + value list within the node
- Intra-node search
  - Linear
  - Binary
  - Interpolation: appr. location of desired key based on know key distribution
-  Optimiztions
  - Prefix Compression: Sorted key are likely to have same prefix, extract common prefix and store unique suffix
  - Deduplication: could have multiple copies of same keys, only maintain distinct values.
  - Bulk Insert: fastest way to build tree is sort table and build tree from bottom.
