

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


