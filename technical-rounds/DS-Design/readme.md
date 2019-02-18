
### 1. Max stack

- Maintain 2 stacks max and regular.
- push:
  - If current>max.peek(), push current in both max and regular
  - Otherwise push current in regular, push max.peek() in max
- pop
  - pop both stacks
- pop_max
  - x = max.peek()
  - initialize buffer stack
  - while current.peek() is not x, pop current and push in buffer, pop max
  - if current.peek() is x, pop current, max, perform push operation by pushing buffer.pop


### 2. LRU Cache

- We must support fast lookups and fast removals
- Have linked list with custom_node
- Have hashmap with <Key, custom_node>
- Each time we get(Key), use hashmap to retrieve node and add it to front
- When we have to remove, simply remove last node of list

### 3. Track largest k elements seen so far from a data stream

- Maintain a min heap
- if element x > pq.peek()
- pq.pop(); insert x


### 4. Moving average of sliding window

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window. (leetcode 346)

- Maintain queue of size n (n=window size)
- Keep track of sum of elements and size in queue
- Get average accordingly

### 4. Median of Data Stream

The median is as follows:

- Sort the array, return middle element or middle 2 elements
- So if you divide the array into 2 parts after sorting, the elements we are interested in are:
  - Largest element in the first half
  - Smallest element in the second half
- Hence you can create 2 heaps for this. First heap tracks largest element in 1st half and the second tracks smallest element in the 2nd half.
- If first element add to max heap
- Else check if element is less than contents of max heap
  - If yes, add it to max heap
  - Else add it to min heap
- If on adding, max-min > 1, then add top value of larger heap to other heap
