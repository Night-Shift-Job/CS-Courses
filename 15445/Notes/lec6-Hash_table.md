# Access Data from Page: Hash Tables
Hash function to compute offset into array for a given key.  
Space complexity: O(n); Time complexity: avg O(1), worst O(n);

## Static Sized Hash
For any input key, return an integer representation of that key.

### Linear Probe Hashing
Single giant table of slots.  
Resolve collisions by linearly searching for the next free slot in the table.
- Insert: hashing to target location and scan for free slot to store
- Search: hashing to target and scan; return when find or none when meet free slot
- deletion left blank slot, failing scan
  - tombstone: insert and search skip
  - movement afterwards slot

- None-Unique keys
  - Separate Linked List - store values in separate storage area
  - Redundant keys - store duplicate keys entries together in hash table

### Robin Hood Hashing
Variant of linear prob hashing that *steals slots from "rich" keys and give to "poor" keys*.
- Each key tracks # of positions they are from where its optimal position in the table.
- On insert, a key takes the slot of another key if the first key is farther away from its optimal position than second key.

### Cuckoo Hashing
Multiple hash tables with different hash function seeds.
- On insert check every table and pick one with free slot.
- If no table has free slot, evict element from one of them and rehashing.

## Dynamic Hashing
### Chained Hashing
A linked list of bucckets for each slot in hashing table.  
Placing all elements with same hash key into same bucket.

### Extendible Hashing
Reshuffle bucket entries on split and increase number of bits to examine.

### Split pointer
- Split pointer to next bucket to split. **Any** bucket overflows, split bucket at pointer location
- Use multiple hashes to find right bucket for given hash.
- When split pointer reaches end, drop first hash function and start over.
