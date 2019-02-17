
### 1. Remove duplicates from a linked list

Logic: Use hashtable/hashset to see if something has been visited already if true, remove duplicate

Follow up: What if additional data structures are not allowed?
  - In that case for every element check every other element in O(n^2)

### 2. Return kth to last node

Logic:
- Have 2 pointers p1, p2
- Move p2 to p2.next k-1 times
- While p2.next is not null
  - p1 = p1.next and p2 = p2.next
- Delete p1

### 3. Delete middle node of linked list

Logic: 
- Have 2 pointers p1, p2 starting at node 1
- while p2.next is not null and p2.next.next is not null
  - p1 = p1.next
  - p2 = p2.next.next
- Delete p1

### 4.Partition: Partition a linked list around value x - here all values greater than x should be after all elements less than x.

Approach 1:

- Create 2 lists. 1 list contains elements less than x, other list contains elements greater than x
- Combine l1 and l2.

Approach 2: Quicksort partition

Quicksort algorithm:
- i=-1, j=0
- j goes from 1 to len-1 (assuming len is pivot)
- if j> pivot, do nothing and increment j
- if j<pivot
  - increment i
  - swap i,j values
  - increment j and continue
- at the end
  - increment i
  - swap i, pivot values
  - end

Code:

    private static void partition(int[] a) {
      int p = a.length-1;
      int i = -1;
      int j = 0;
      while(j!=p) {
        if(a[j] > a[p])
          j++;
        else {
          i++;
          int temp = a[i];
          a[i] = a[j];
          a[j] = temp;
          j++;
        }
      }
      i++;
      int temp = a[i];
      a[i] = a[p];
      a[p] = temp;
    }

### 5. Sum lists

### 6. Palindrome: Check if a linked list is a palindrome

Logic: 
- Push first half of the list into a stack
- compare values by popping stack

### 7. Given 2 linked lists, check if they intersect

Logic:

- let l1 be length of list1, l2 be length of list2. Let p1 be start of list1 and p2 be start of list2
- let d be difference between l1 and l2. increment pointer of longer list d times
- while(p1 is not null)
  - check if p1 = p2. if true return true
  - else increment p1, p2
  - return false after while loop ends

### 8. Loop detection in linked list

1. Check if there exists a loop

- fast, slow = head
- while fast.next is not null and fast.next.next is not null
  - slow = slow.next, fast = fast.next.next
  - If slow = fast there is a loop
- If while loop terminates there is no loop

2. When do slow and fast collide?

- K steps before start of loop

3. how to find start of loop

- node head and collision spot are both equidistant from the loop
- Have pointer p1 at head, p2 at collision point
- increment both pointers until p1 = p2 and you have collision point.
