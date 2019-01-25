

# Big O Notation

The Big O notation describes how quickly a probram runs and how much space the program needs. This is called Time/Space complexity.

- It is calculated **relative to the input size** which is denoted as 'n'
- It approximates how quickly the runtime grows
- It is helpful for approximating estimates for large inputs.

In the notation only the most significant term matters. So if we have n^2 + n, the complexity would be O(n^2). We are usually considering the worst case. In some cases there is a tradeoff between saving space and time.

# The Basics

## Memory

A computer has RAM (memory) and some Storage (disk). The computer must keep track of variables (integers, strings, arrays etc) and this is stored in the RAM. Memory is where we keep our variables, storage is where we keep our files.

- Think of RAM as a hashmap/array where the key is an address and the value is a byte (8 bits).
- The processor does the real work inside a computer. The processor is connected to a memory controller that reads/writes bytes to the RAM
- The Memory Controller has direct access to every element of the RAM so it can access address 1 and then address 9999 without having to go through the addresses in between.

However even though the memory controller can jump between addresses it is still faster if things are stored sequentially. This is because: 

- The processor has a cache that is much faster than the RAM. The cache stores data most recently read from the RAM
- When the processor asks for contents of a given memory address, the Memory Controller also sends the nearby memory addresses to the cache.
- Because sequential data is stored in the cache, it is faster to access than jumping around via the Memory Controller.

## Binary Numbers

In computers data is stored in binary format. Binary is base 2 and can only have 2 values - 1 and 0. Here is how numbers are stored:

- Unsigned positive numbers: Stored as usual in the form of base 2 0s and 1s
- Signed numbers: Reserve the leftmost bit for the sign. 0 means positive, 1 means negative.
- Fractions: Stored as 2 numbers - numerator and denominator
- Decimals: Stored as 2 numbers - the number without the decimal and the position of the decimal (number of digits from leftmost digit after which the decimal must be inserted).

Since each bit can store 2 values, 8 bits can store 2^8 = 256 values. We usually use 4-8 butes (32 or 64 bits to store inegers). In Java int = 32 bits, Long = 64 bits.

  - Sometimes number of bits is not enough for example youtube used 32 bit signed ints to count views however when Gangam Style hit 2^31 views it ran into trouble and had to upgrade to 64 bit ints

Most integers are fixed length (unless it is a big-int) hence regardless of value, it always takes same amount of space in th ram and hence they are O(1) or constant space. 

Also because they have a constant number of bits, most simple operations like addition, subtraction, multiplication and division take O(1) or constant time.

## Data

### Arrays

Pros: Fast lookup
Cons: Each item needs to be the same size; Since arrays are contiguous, you need a block of uninterrupted free memory to store it - this is hard when the array gets big.

### Strings

A string is a sequence of characters that can be stored in an array. But how can an array store characters instead of numbers? This is done by defining a mapping between numbers and characters also known as character encoding. One such encoding is ASCII where:

- A: 01000001; B: 01000010; C: 01000011 and so on

So how does a computer know if a sequence such as '01000001' and 1s is ASCII or just a regular number? It does not know. Not on its own. Software running on the computer imposes structure on binary data and this structure is what allows the software to interpret various blobs of data as letters, numbers, pictures etc.

### Pointers

If we want to store variable length items in the array we can store pointers to the memory addresses of the items instead. However doing so does not make the array cache friendly as pointers point to stuff that is scattered all over the RAM.

### Dynamic Array/Arraylist

We normally declare the size of the array so that the computer can reserve space in the memory. However we can get past this by declaring an arraylist.

Here after the array increases by a certain amount, a new array 2x the size of the original one is created and the contents of the original array are copied into it. This copy operation is O(n) time.

Pros: We do not have to bother with the size
Cons: While most appends are cheap O(1), certain appends are expensive O(n) for the copy operation after limit is exceeded.

### Lists

Lists consists of interconnected nodes that store some data and a pointer to the next node.

Pros: Fast O(1) inserts
Cons: Slow O(n) lookups as we need to iterate upto the position; Additionally since nodes may have arbitrary memories, it is not cache friendly.

### HashMaps

Stores Key/Value pairs. If we want to store a value, we perform some arithmetic operation on it and that becomes our key. In that key address, we store our desired value.

If 2 values end up having the same key, we call it a collision. One way to resolve a collision is to make the value into a list of values pointed by the key. To know which value in the list we are referring to, we store additional information in each node of the list that helps us identify stuff.

For example: 

- If we have Map <String, Integer>. 
- We have 2 key value pairs ABC: 8, XYZ: 10
- If both ABC and XYZ hash to the same memory address we maintain a list.
- This list does not contain just the value - it contains the key as well
- So if we do map.get(XYZ) it will search through the list and get the node with XYZ.
- Hence it will return 10 and NOT 8.

In practice collisions are rare and lookups are considered to be O(1) time.

# Logarithms
