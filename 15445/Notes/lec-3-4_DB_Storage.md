# Storage Access
DBMS want to maximize sequential access
- reduce number of write to random page
- allocate multiple page continuously at the same time

## Why not use OS?
Page fault might happen when multiple threads try to write.
And db could do better:
- Flush dirty page to disk in **correct order**
- Specialized prefetching
- Buffer replacement policy
- Thread/Process schedualing

## DB representation in disk
### File Storage
files organized as a collection of **pages**
#### Page
- fixed size block of data
- most system do not mix page types
- each page has unique identifier: pageid

- Heap File - unordered collection of pages that stored in random order
  - support CGWD Page
  - support iterate over all pages
  - keep track of what page exist in multiple files and which one have free space
  - Two ways represent heap file
    - Linked List: maintain a header page at beginning with two pointers HEAD of free page list and HEAD of data page list; each page keep track of free slot they current have
    - Page Directory: maintain special pages track location of data pages in database files, and record # of free slot per page

### Page Layout
Every page contains a metadata about page content.
- tuple oriented: slotted pages, slot array after header that map tuple to position offsets, and tuple grows from page end to start.
  - record ids: keep track individual tuples, page_id + offset/slot
  - Tuple Layout: A sequence of bytes.
    - header of metadata: visibility info(concurrency control), bit map for null values.
    - Attr stored in order when create or lay differently.
    - denormalize related tuples and store in same page, reduce IO but make update more expensive.
  - Log-structured Layout: log record of how db modified;
    - record id, and compact periodically

## Data Representation
### Schema
DBMS **catalogs** contains **schema** about tables that figure out tuple's layout.
- Large values: stores value exceed a page, use seperate overflow page, connect to slot tuple.
- Really large value: blob type, dbms could not manipulate the contents of external file.

### Catalogs
stores meta-data about db
- Tables, columns, indexes, views
- Users, permissions
- Internal statistics

## Data storage model
- NSM: row
  - advantage
    - fast inserts, updates and deletes
    - good for query needs entire tuple
  - disadvantages
    - not good for scanning large portions of table or subset of attrs
- DSM: column
  - identify a tuple:
    - fixed-length offset
    - embedded tuple ids
  - advantage
    - reduces # of wasted IO
    - better query processing and data compression
  - disadvantages
    - slow for point query, inserts, updates and deletes
