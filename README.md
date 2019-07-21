# multihash-tree
Multihash tree spec
https://github.com/multiformats/multihash/issues/55

## Merkle Hash Tree multihash

`<merkle-hash-tree-mc>-<len>-(<hash-tree-type>-<hash-mc>-<hash-len>-(<hash-value>)-[<hash-value>...])`
`<0x0400>-<len>-(<0x01>-<hash-mc>-<hash-len>-(<hash-value>)-[<hash-value>...])`

0x0400 - Merkle Hash Tree (1024 decimal) multicodec value 

For types (01-03):
* Data segment size - 1024 bytes
* Data prefix - one byte with value 0 (0x00)
* Hash pair prefix - one byte with value 1 (0x01)
* Unpaired hashes move on next level unchanged.

## Tree included in multihash:

Small tree can be included to multihash. This can be used in blocks.

0x01 - Hash Tree Type with this settings:
* hashes in tree packed in breadth-first order from root to leafs.
```
Level           Tree Root
|                   |
V                   V  
1:            _____ 1 _____ <-------
             /             \         \
2:       ____2____           3 <---- Internal Hashes
        /         \           \          /
3:     4           5           6 <------/
      /  \       /   \        / \      /
4:   7    8     9     10    11  12 <--
    /\    /\    /\    /\    /\   \
5: 13 14 15 16 17 18 19 20 21 22 23 <---- Leaf Hashes
   |___________ data _____________|
```
Order: root hash 1  (1 level); hash 2, hash 3  (2 level); hash 4, hash 5, hash 6 (3 level);

0x02 - Hash Tree Type with this settings:
* One level hash tree
  1. level - 1 hash = root-hash
  2. level - 2 hashes
  3. level - 3-4 hashes
  4. level - 5-8 hashes
and so on

Order: hash 7, hash 8, hash 9, hash 10, hash 11, hash 12 (4 level);

Root hash buils from this hash slice.

## Hash chain:

To allow peer request part of block it must send hash pairs chain from root to part. Last hash is requisted part.

0x03 - Hash chain with this settings:
* Last hash in chain is the requisted block part. Seed must turn hash pairs until it hash match with last hash from upper level to find part position in block. 

Order: root hash 1 (1 level); hash 3, hash 2 (2 level); hash 5, hash 4  (3 level);

Requested block with hash 4

## Hashlist request/responce

For types (05-06):
* Block is hash list
* Size of each hash in hash list same as in root multihash
* Type of hash same as in root multihash

Hashlist request/responce contain only root hash in multihash. On that request seed send hashlist block in format:

0x05 - hash list block with hash tree packed in breadth-first order from root to leafs.

0x06 - hash list block with one level of hash tree.

## Tiger multihash:

`<tiger-mc>-<len>-<hash>`
`<0x7A>-<0x18>-<0x3293ac630c13f0245f92bbb1766e16167a4e58492dde73f3>`

0x7A - Tiger-hash multicodec value

## Tree Tiger multihash:
`<merkle-hash-tree-mc>-<len>-(<hash-tree-type>-<tiger-mc><tiger-len>(<tiger-hash>)[<tiger-hash>...])`
## Tree sha2-256 multihash:
`<merkle-hash-tree-mc>-<len>-(<hash-tree-type>-<sha2-256-mc>-<sha2-256-len>-(<sha2-256-hash>)[<sha2-256-hash>...])`

## Links:
1. [Tree Hash EXchange format (THEX)](https://adc.sourceforge.io/draft-jchapweske-thex-02.html#anchor2)
2. [Tiger:
A Fast New Cryptographic Hash Function (Designed in 1995)](http://www.cs.technion.ac.il/~biham/Reports/Tiger/)
3. wikipedia: [Tiger (cryptography)](https://en.wikipedia.org/wiki/Tiger_(cryptography))
4. wikipedia: [Merkle tree#Tiger tree hash](https://en.wikipedia.org/wiki/Merkle_tree#Tiger_tree_hash)
