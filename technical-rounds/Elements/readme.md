

# Primitives

### Count number of 1s in a number

Idea: binary of 1 is 00001. So if the rightmost bit has 0, the and of 1 and the number is 0 otherwise it is 1. So shift all bits of x right one by one and and it with 1. Add the result to a counter.

    private static int f(int x) {
      int count = 0;
      while(x!=0) {
        count = count + (x & 1);
        x = x >>> 1;
      }
      return count;
    }
    
### Compute the Parity of a word

If number of 1s is odd, parity = 1 else it is 0.

### Closest int with same weight

### Compute x^y

### Reverse digits

Given -314, answer should be -413

    public int reverse(int x) {
        boolean flag = false;
        if(x < 0) {
            flag = true;
            x = x*-1;
        }
        long result = 0;
        while(x > 0) {
            result = result*10 + x%10;
            x = x/10;
        }
        if(flag)
            result = result*-1;
        if(result > Integer.MAX_VALUE || result < Integer.MIN_VALUE)
            return 0;
        return (int) result;
        
    }

### Generate Random numbers


### Rectangle Intersection

# Arrays

### Given array of integers rearrange it in constant space such that odd numbers are after even numbers

    int[] a = {1,2,3,4,5,6};
      int l = 0; int r = a.length-1;
      while(l < r) {
        if(a[l]%2 == 0)
          l++;
        else {
          int temp = a[l];
          a[l] = a[r];
          a[r] = temp;
          r--;
        }
      }

### Dutch National Flag Problem

- Write a partition function for quicksort.

Logic: set i = -1, j = 0. If j>pivot, increment j. Otherwise swap i,j values and increment i,j.

      static void partition(int[] a) {
          int p = a.length-1;
          int i = -1;
          int j = 0;
          while(j < a.length-1) {
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
        
This is a 2 way partition. Now we will look at a 3 way partition. The problem is as follows:

- Given array of 0,1,2 sort it in 1 pass

      static void partition(int[] a) {
        int s=-1;
        int m=0;
        int l = a.length-1;

        while(m<l) {
          if(a[m] < 1) {
            s++;
            int temp = a[s];
            a[s] = a[m];
            a[m] = temp;
            m++;
          } else if(a[m] == 1) {
            m++;
          } else {
            int temp = a[l];
            a[l] = a[m];
            a[m] = temp;
            l--;
          }
        }
      }

### Jump game

See leetcode

    public boolean canJump(int[] a) {
        int max = 0;
        for(int i=0; i<a.length; i++) {
            max = Math.max(max, i+a[i]);
            if(max >=a.length-1)
                return true;
            if(i==max)
                return false;
        }
        return false;
    }
    
Variation: Jump Game 2 (based on bfs)

    public int jump(int[] a) {
        Map<Integer, Integer> map = new HashMap<>();
        Queue<Integer> q = new LinkedList<>();
        map.put(0,1);
        q.add(0);
        while(!q.isEmpty()) {
            int i = q.remove();
            int j = i;
            while(j<=a.length-1 && j <= i+a[i]) {
                if(!map.containsKey(j)) {
                    map.put(j, map.get(i) + 1);
                    q.add(j);
                    if(j==a.length-1)
                        return map.get(i);
                }
                j++;
            }
        }
        return 0;
    }

### Delete duplicates from sorted array

    public int removeDuplicates(int[] a) {
          int i=0;
          int j = 1;
          int count = 0;
          while(j<a.length) {
              if(a[i] == a[j])
                  j++;
              else {
                  i++;
                  count++;
                  int temp = a[i];
                  a[i] = a[j];
                  a[j] = temp;
                  j++;
              }
          }
          return count+1;
      }

- Variant: Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

