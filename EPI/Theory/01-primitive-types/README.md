Note: read this https://catonmat.net/low-level-bit-hacks

TO-DO: 

- learn how arithmetic operations are done at the bit level
- watch this playlist: https://www.youtube.com/watch?v=QFXaddi-Ag8&list=PLEzDT-amkjJd_Kel3iUjGsTR168NecVzc

# Primitive Types

## Bit manipulation

--------------------------------------------

### Positive and Negative Integers

In Java, integers are stored as **32 bit values** of 0s or 1s.

- 0 is represented as `00000000000000000000000000000000`
- 1 is represented as `00000000000000000000000000000001`
- and so on...

Here the leftmost bit also known as the **most significant bit** denotes the sign.

- Anything that starts with a 0 is a **positive** number
- Anything that starts with a 1 is a **negative** number

To **change a sign** of a number do the following:

- Convert all the 0s to 1s and all the 1s to 0s
- The add 1 to get the negative number.
- Let us change a positive number to a negative number
    - The number 1 is: `00000000000000000000000000000001`. Let's convert it to -1
    - Change all the 0s to 1s and all the 1s to 0s. This becomes `11111111111111111111111111111110`
    - Now add 1 to it. It becomes `11111111111111111111111111111111`. This is binary of -1
- Let us change a negative number to a positive number
    - The number -3 is: `11111111111111111111111111111101`. Let's convert it to 3
    - Change all the 0s to 1s and all the 1s to 0s. This becomes `00000000000000000000000000000010`
    - Now add 1 to it. It becomes `00000000000000000000000000000011`. This is binary of 3

----------------------------------------------

### Basic Operators

The basic operators are:

- AND (&): 1&1 = 1, 0&1 = 0, 0&0 = 0
- OR (|): 1|1 = 1, 1|0 = 1, 0|0 = 0
- NOT(~): Converts 0s to 1s and 1s to 0s
    - Note that ~0 = -1 because 0 gets converted to `11111111111111111111111111111111`
- XOR (^): 1^1 = 0, 1^0 = 1, 0^0 = 0

----------------------------------------

### Shifting bits

We can shift bits to the left or to the right.

- To **shift bits left** we can use `<<`. 
    - For example `0010 << 1` shifts the bits once to the left. It becomes `0100`.
    - Note that **shifting left by 1** is the equivalent of **multiplying by 2**
- To **shift bits right** we can use `>> or >>>`
    - For example `0010 >> 1` shifts the bits once to the right. It becomes `0001`
    - Note that **shifting right by 1** is the equivalent of **dividing by 2**

Let us consider the case of **right shift**. Here we have either `>> or >>>`. These are the arithmetic and logical right shifts.

- `>>` is the **arithmetic right shift**. Every time we shift right, **the value it inserts is the value of the signed bit**.
    - For example if we have x = -32 `11111111111111111111111111100000` 
    - Doing x >> 1 gives us -16 `11111111111111111111111111110000`. Since the sign bit was 1, it was the value inserted.
- `>>>` is the **logical right shift**. Every time we shift left, **the value it inserts is 0**.
    - For example if we have x = -32 `11111111111111111111111111100000`
    - Doing x >>> 1 gives us 2147483632 `01111111111111111111111111110000`. Here the new bit inserted was 0

Note that **java does not have logical left shift**. This is because logical left shifts are redundant. We **always insert 0 for left shifts** and it makes no sense inserting a 1 so unlike right shifts, in left shift the logical left shift is not needed.

---------------------------------------------

### Bit masks

Bit masking is the act of performing a boolean operation on a number to get our desired result. Consider the example shown below:

- Let's say we have `01010000101010101`. 
- Our desired result is `01010000000000000` - we want to get a result where the only 1s come from the 4 leftmost digits of the original number
- To get our desired result we can simply do `01010000101010101` (original number) & `11110000000000000`
- This operation gives us `01010000000000000` (desired result)

Here the number we used to get the result `11110000000000000` is the **bit mask**

Since bit masks can isolate chunks of the input, they are often used in solutions that make use of hashmaps to perform repeated operations quickly.

------------------------------------------------

### Lowest set bit and lowest bit not set

It is important to memorize ways to find the lowest set bit and the lowest bit that is not set as it can help in several bit manipulation problems. Consider the following table:

    ----------------------------------------------
    x           |   0   0   1   0   1   1   0   0
    ----------------------------------------------
    (x-1)       |   0   0   1   0   1   0   1   1
    ----------------------------------------------
    ~(x-1)      |   1   1   0   1   0   1   0   0
    ==============================================
    x & (x-1)   |   0   0   1   0   1   0   0   0
    ----------------------------------------------
    x & ~(x-1)  |   0   0   0   0   0   1   0   0
    

From the table, we can make the following observations:

- The result of **x & (x-1) drops the least significant set bit** of x
- The result of **x & ~(x-1) isolates the least significant set bit** of x

Similarly consider the following table:

    ----------------------------------------------
    x           |   0   0   1   0   1   1   0   0
    ----------------------------------------------
    ~x          |   1   1   0   1   0   0   1   1
    ----------------------------------------------
    (x+1)       |   0   0   1   0   1   1   0   1
    ----------------------------------------------
    ~(x+1)      |   1   1   0   1   0   1   0   0
    ==============================================
    ~x & (x+1)  |   0   0   0   0   0   0   0   1
    
From the table we can make the following observation:

- The result og ~x & (x+1) isolates the first bit that is not set

We can use this in any problem where we have to count the number of set bits or find the least significant set/not set bit.

---------------------------------------------------

### Associative Property and Commutative Property

These properties can be used to quickly perform some operations parallelly or by reordering input in certain ways. These properties are described below:

- The **Associative Property** says that **(a+b)+c == a+(b+c)**
- The **Commutative Property** says that **a+b == b+a**

These properties can be applied to boolean operations as well.

To understand how they speed up certain operations let us consider the following problem: **Find the parity of a binary string**. If the string has odd number of 1s the parity is 1 else it is 0.

- Normally we would have solved this problem by counting the number of 1s. 
- Even if we skip all 0s and directly count the 1s using the **lowest set bit** trick in the previous section, the worst case complexity is still n.
- We can use the **Associative and Commutative properties** to reduce the complexity to **log(n)**.

Consider the string `11010111`. Let us find the parity of that.

- The parity od `11010111` is equal to parity of `1101 XOR 0111`. This is `1010` now we must find its parity
- The parity of `1010` is equal to parity of `10 XOR 10`. This is `00`
- The parity of `00` is equal to the parity of `0 XOR 0`. This is `0`

Hence we can conclude that the given number has an even parity. 

Note that we can perform the XOR of the 2 parts by simply **right shifting the number (n/2) times**.

- `11010111 >>> 4` equals `00001101`. Now we can consider only the 4 rightmosr digits of the 2 values in our XOR which is `0111` and `1101`. 
- The value of `11010111 ^ 00001101` is `11011010`. We are interested only in the last 4 digits `1010` to continue the operation. So we right shift by 2 and so on.
