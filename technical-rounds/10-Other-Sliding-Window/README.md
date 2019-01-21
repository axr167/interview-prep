
# Overview

A sliding window is a list of fixed or variable length that iterates over a collection or a string. It represents a continuous block or collection of data within the collection/string that contains all the information we need.

The idea is to have a left and a right pointer to represent the 2 ends of the window. Both the pointers point to a certain index of the collection/string. The left pointer can never be greater than the right pointer - it must be less than or equal to it. We then iterate over the elements of the collection until the right pointer reaches the end.

The sliding window technique must be used when:

- We are tasked with finding a continuous block of data like a substring of a string or a continuous subarray
- That substring/subarray must satisfy certain conditions

The way we do this is:

- Initialize the start and end pointer to the 0th element. Elements in the window are all elements between (end-1)-start. When start = end no element exists in window.
- Initialize a helper data structure such as a hashmap to keep track of the conditions that need to be satisfied. Also initialize a variable to keep track of the current best result.
- Iterate through the elements using the window
- If all conditions are satisfied or if the condition cannot be satisfied: 
  - Move start pointer towards end pointer gradually removing corresponding element from data structure until either condition is satisfied again or until start = end
- When we reach the end of the collection, we should have an answer.
