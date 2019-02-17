
### 1. Implement 3 stacks with 1 array

If stack is fixed size:

- Divide stack into n/3 parts
  - S1 = 0...n/3-1
  - S2 = n/3....2n/3-1
  - S3 = 2n/3...n

If stack is not fixed size:

- Use arraylist
- 0...9 = S1
- 10...19 = S2
- 20...29 = S3
- 30...39 = S1 and so on

### 2. Design a stack that supports peek_min and pop_min operations.

Logic: For every element pushed keep track of min in second stack

In leetcode we have same thing but for max element. The logic and code is:

- Push:
  - If both stacks are empty push element to both stacks
  - If x > stack.top, push element to both stacks
  - Otherwise push x to s1 and push s2.peek() to s2

- Pop
  - Pop both s1 and s2

- Pop_Max
  - While s1.top is not max, pop both s1 and s2 and store s1 in buffer stack
  - Then pop s1 and s2
  - Then push buffer contents (according to push operation mentioned above)
  

Code (716 leetcode):

    class MaxStack {

        /** initialize your data structure here. */
        Stack<Integer> s1;
        Stack<Integer> s2;

        public MaxStack() {
            s1 = new Stack<>();
            s2 = new Stack<>();
        }

        public void push(int x) {
            if(s1.isEmpty() || x >= s2.peek()) {
                s1.push(x);
                s2.push(x);
            } else {
                s1.push(x);
                s2.push(s2.peek());
            }
        }

        public int pop() {
            s2.pop();
            return s1.pop();
        }

        public int top() {
            return s1.peek();
        }

        public int peekMax() {
            return s2.peek();
        }

        public int popMax() {
            Stack<Integer> buffer = new Stack<>();
            int max = s2.peek();
            while(s1.peek()!=max) {
                buffer.push(s1.pop());
                s2.pop();
            }
            s1.pop();
            s2.pop();
            while(!buffer.isEmpty())
                push(buffer.pop());
            return max;
        }
    }

### 3. Stack of plates

See CTCI

### 4. Queue using stacks

Logic:

- Have 2 stacks old, new.
- Each time you push, add to new.
- Each time you pop, pop from old. If old is empty pop new and push it into old (this reverses the value making it fifo)

Code (232 leetcode):

    class MyQueue {

        /** Initialize your data structure here. */
        Stack<Integer> oldStack;
        Stack<Integer> newStack;

        public MyQueue() {
            oldStack = new Stack<>();
            newStack = new Stack<>();
        }

        /** Push element x to the back of queue. */
        public void push(int x) {
            newStack.push(x);
        }

        private void exchange() {
            while(!newStack.isEmpty()) {
                oldStack.push(newStack.pop());
            }
        }

        /** Removes the element from in front of queue and returns that element. */
        public int pop() {
            if(!oldStack.isEmpty())
                return oldStack.pop();
            else
                exchange();
            return oldStack.pop();
        }

        /** Get the front element. */
        public int peek() {
            if(!oldStack.isEmpty())
                return oldStack.peek();
            else
                exchange();
            return oldStack.peek();
        }

        /** Returns whether the queue is empty. */
        public boolean empty() {
            if(oldStack.isEmpty() && newStack.isEmpty())
                return true;
            return false;
        }
    }

### 5. Sort Stack

### 6. Animal Shelter
