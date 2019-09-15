
## Primitive Types

### Bit manipulation
---------------------------------------------

**Positive and negative integers**

In Java, integers are stored as 32 bit values of 0s or 1s.

- 0 is represented as 00000000000000000000000000000000
- 1 is represented as 00000000000000000000000000000001
- and so on...

Here the leftmost bit (also known as the most significant bit) denotes the sign.

- Anything that starts with a 0 is a positive number
- Anything that starts with a 1 is a negative number

To change a sign of a number do the following:

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

**Basic operators**

The basic operators are:

- AND (&): 1&1 = 1, 0&1 = 0, 0&0 = 0
- OR (|): 1|1 = 1, 1|0 = 1, 0|0 = 0
- NOT(~): Converts 0s to 1s and 1s to 0s
    - Note that ~0 = -1 because 0 gets converted to 11111111111111111111111111111111
- XOR (^): 1^1 = 0, 1^0 = 1, 0^0 = 0

