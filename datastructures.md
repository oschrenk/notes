# Data Structures #

## Probabilistic  ##

**Bloom Filter**. A Bloom filter is a data structure designed to tell you, rapidly and memory-efficiently, whether an element is present in a set.
The price paid for this efficiency is that a Bloom filter is a probabilistic data structure: it tells us that the element either definitely is not in the set or may be in the set. You ask the datastructure if an element exists. False positives are possible. You may get `true` for elements that are not in the set. False negatives are not possible. You never get `false` for elements that are in the set.

**Skip Lists**. A skip list is a data structure for storing a sorted list of items using a hierarchy of linked lists that connect increasingly sparse subsequences of the items. These auxiliary lists allow item lookup with efficiency comparable to balanced binary search trees (that is, with number of probes proportional to log n instead of n). Skip lists are also used in distributed applications (where the nodes represent physical computers, and pointers represent network connections) and for implementing highly scalable concurrent priority queues with less lock contention, or even without locking, as well lockless concurrent dictionaries.

Scott Vokes, http://www.infoq.com/presentations/Data-Structures

## Hashing ##

**Rolling Hash**. A rolling hash is a hash function where the input is hashed in a window that moves through the input. A popular application is rsync program.

## Treeish ##

- [Judy Array](http://judy.sourceforge.net/) Sparse dynamic array, consumes memory only when it is populated.

## Sources ##

- [Bloom Filter](http://llimllib.github.com/bloomfilter-tutorial/)
- [Skip Lists](http://en.wikipedia.org/wiki/Skip_list)