
# Divide and Conquer

## Overview

Divide and conquer is basically this:
- Split the input 
- Compute the best result for the inputs that have been split
- The answer is either:
  - The better answer of the 2 parts
  - The answer created by joining the 2 parts in some way.

## Complexity

The complexity of divide and conquer algorithms can be figured out using the Master theorem.

![Master-Theorem](https://i.imgur.com/6KqqNhC.png)

### How this theorem works

We split the input into a set number of parts until the input can no longer be split. Then we perform certain operations on the split parts to get the result.

Let us consider a few examples.


**Find maximum element in array**

We split array into 2 parts then for both parts we get the max element and return overall max.

- T(n) = 2 * T(n/2) + n^0

Here 0 < log_2(2) hence T(n) = n^log_b(a) => n^log_2(2) => n

**Mergesort**

We split array into 2 parts then for both parts we merge, processing elements one after the other

- T(n) = 2 * T(n/2) + n^1

Here 1 = log_2(2) hence T(n) = n^d log(n) = nlog(n)

# Problems

### Get max element of an array

### MergeSort

### Buy/Sell stocks

### Get maximum subarray

### Eugene's question (see youtube vid)

### Largest area under histogram
