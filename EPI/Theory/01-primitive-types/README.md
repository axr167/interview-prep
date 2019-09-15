
# Primitive Types

## Bit manipulation

--------------------------------------------

### Positive and Negative Integers

In Java, integers are stored as **32 bit values** of 0s or 1s.

- 0 is represented as 00000000000000000000000000000000
- 1 is represented as 00000000000000000000000000000001
- and so on...

Here the leftmost bit also known as the **most significant bit** denotes the sign.

- Anything that starts with a 0 is a positive number
- Anything that starts with a 1 is a negative number

To **change a sign** of a number do the following:

- Convert all the 0s to 1s and all the 1s to 0s
- The add 1 to get the negative number.
- Let us change a positive number to a negative number
    - The number 1 is: 00000000000000000000000000000001. Let's convert it to -1
    - Change all the 0s to 1s and all the 1s to 0s. This becomes 11111111111111111111111111111110
    - Now add 1 to it. It becomes 11111111111111111111111111111111. This is binary of -1
- Let us change a negative number to a positive number
    - The number -3 is: 11111111111111111111111111111101. Let's convert it to 3
    - Change all the 0s to 1s and all the 1s to 0s. This becomes 00000000000000000000000000000010
    - Now add 1 to it. It becomes 00000000000000000000000000000011. This is binary of 3

----------------------------------------------

### Basic Operators

The basic operators are:

- AND (&): 1&1 = 1, 0&1 = 0, 0&0 = 0
- OR (|): 1|1 = 1, 1|0 = 1, 0|0 = 0
- NOT(~): Converts 0s to 1s and 1s to 0s
    - Note that ~0 = -1 because 0 gets converted to 11111111111111111111111111111111
- XOR (^): 1^1 = 0, 1^0 = 1, 0^0 = 0

----------------------------------------

### Shifting bits

We can shift bits to the left or to the right.

- To **shift bits left** we can use **<<**. 
    - For example **0010 << 1** shifts the bits once to the left. It becomes **0100**.
    - Note that **shifting left by 1** is the equivalent of **multiplying by 2**
- To **shift bits right** we can use **>> or >>>**
    - For example **0010 >> 1** shifts the bits once to the right. It becomes **0001**
    - Note that **shifting right by 1** is the equivalent of **dividing by 2**

Let us consider the case of **right shift**. Here we have either **>> or >>>**. These are the arithmetic and logical right shifts.

- **>>** is the **arithmetic right shift**. Every time we shift right, **the value it inserts is the value of the signed bit**.
    - For example if we have x = -32 (11111111111111111111111111100000) 
    - Doing x >> 1 gives us -16 (11111111111111111111111111110000). Since the sign bit was 1, it was the value inserted.
- **>>>** is the **logical right shift**. Every time we shift left, **the value it inserts is 0**.
    - For example if we have x = -32 (11111111111111111111111111100000)
    - Doing x >>> 1 gives us 2147483632 (01111111111111111111111111110000). Here the new bit inserted was 0

Note that **java does not have logical left shift**. This is because logical left shifts are redundant. We **always insert 0 for left shifts** and it makes no sense inserting a 1 so unlike right shifts, in left shift the logical left shift is not needed.

---------------------------------------------

### Bit masks

Bit masking is the act of performing a boolean operation on a number to get our desired result. Consider the example shown below:

- Let's say we have **01010000101010101**. 
- Our desired result is **01010000000000000** - of the 4 leftmost digits we want to get all the 1s
- To get our desired result we can simply do **01010000101010101** (original number) & **11110000000000000**
- This operation gives us **01010000000000000** (desired result)

Here the number we used to get the result **11110000000000000** is the **bit mask**
