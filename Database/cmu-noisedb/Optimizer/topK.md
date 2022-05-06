# core method
- [x] Increment
- [x] Decrement

# How
This feature depends on `count_min_sketch`, which is a wrapper of [`madoka::Sketch`](https://github.com/s-yata/madoka);

It tracks approximate number of each element inside of it.

key - number inside are compact and fast to retrieve.