
## Arrays

Organizes data sequentially in memory. Fiexed size and has fast lookups and appends O(1) but expensive inserts and deletes O(n). They are building blocks for lots of other data structures.

Array slicing involves taking a subset from an array and allocating it to a new array. It is O(n) time and space and can be done as follows: int[] copy = Arrays.copyOfRange(array, 0, array.length);

An **in-place** algorithm is one that operates directly on the input and changes it instead of creating/returning a new object. These are destructive as original input is destroyed. Out of place algirithms are safer as it preserves the input however they consume extra space.

Dynamic arrays are variable size arrays and like its static counterparts it has O(1) lookups and appends but O(n) inserts and deletes. They are also cache friendly. However they have slow worst case appends.
