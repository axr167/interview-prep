
### Hash Functions

A hash function is a method that takes in a given input and outputs a fixed size string/number called the hash or the hash code. A given input will always have the hash code however we cannot derive the input from the hash.

It is used in:

- Hash tables which is used for constant time lookups, inserts and deletes
- Preventing man in the middle attacks: For example some websites store the hash along with the raw file using something like SHA or MD5. After we finish downloading, we hash the file ourselves with SHA/MD5/whatever and check it with the website's hash to confirm that we downloaded the file correctly.

### HashMaps

Supports constant time lookups, inserts and deletes.

Some cons are:

- Slow worst case lookups (if we have too many collisions)
- Keys are unordered so if we want to find min key, we must iterate through the whole hashmap
- Lookups are single directional. Given key we can find corresponding value in O(1) time but given value we can find corresponding key only by iterating over all values in O(n) time.
