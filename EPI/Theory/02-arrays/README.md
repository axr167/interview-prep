
# Array based problems

## Partitioning problems

In these types of problems, we need to partition arrays into 2 (or 3) types where each type has a similar set of elements. The idea here is to use both ends of the array. So we have 3 pointers:

- A left pointer: Everything behind this pointer in the left direction is of type 1.
  - At the start its value = 0 because we have not processed any elements yet hence there is nothing behind this pointer.
- A right pointer: Everything behind this pointer in the right direction is of type 2.
  - At the start its value = a.length -1 because we have not processed any elements yet hence there is nothing behind this pointer.
- The current pointer: This is the pointer that iterates over the array.
