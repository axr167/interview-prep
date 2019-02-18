
### 1. Triple Step: You can jump 1 step, 2 steps or 3 steps at a time. Count no. of ways to run up the steps:

    // f(n) = 1,2,4 //if(n==1,2,3)
    // f(n) = f(n-1)+f(n-2)+f(n-3) //otherwise
    
    private int count(int n) {
      int c1 = 1;
      int c2 = 2;
      int c3 = 4;
      int c;
      for(int i=4; i<n; i++) {
        c = c1+c2+c3;
        int temp = c3;
        c3 = c;
        int temp2 = c2;
        c2 = temp;
        c1 = temp2;
      }
      return c;
    }
    
### 2. Robot in a grid: Min path sum (leetcode)

### 3. Magic index

Q1. Magic index is when a[i] = i in an array. Find it if a has all distinct values and is sorted.

    private static int get(int[] a) {
      int lo = 0;
      int hi = a.length-1;
      while(lo<=hi) {
        int mid = (lo+hi)/2;
        if(a[mid] == mid)
          return mid;
        else if(a[mid]<mid)
          lo = mid+1;
        else
          hi = mid-1;
      }
      return -1;
    }

Q2. What if there are elements are not distinct?



### 4. Power Set

	private static void subsets(StringBuilder a, StringBuilder b, Set<String> set) {
		char[] c = b.toString().toCharArray();
		Arrays.sort(c);
		String sorted = new String(c);
		if(!set.contains(sorted)) {
			System.out.println(b.toString());
			set.add(sorted);
		}
		for(int i=0; i<a.length(); i++) {
			char x = a.charAt(i);
			a.deleteCharAt(i);
			b.append(x);
			subsets(a, b, set);
			a.insert(i, x);
			b.setLength(b.length()-1);
		}
		
	}

### 5. Multiplication without using *

	private static int f(int a, int b, int[] dp) {
		if(dp[b] != 0)
			return dp[b];
		if(b==1)
			dp[b] = a;
		else if(b%2!=0)
			dp[b] = a+ f(a, b/2, dp) + f(a, b/2, dp);
		else
			dp[b] = f(a, b/2, dp) + f(a, b/2, dp);
		return dp[b];
	}

### 6. Tower of Hanoi

### 7, 8. Permutations with/without duplicates

	private static void permute(StringBuilder a, StringBuilder b, int n) {
		if(b.length() == n)
			System.out.println(b.toString());
		for(int i=0; i< a.length(); i++) {
			char c = a.charAt(i);
			a.deleteCharAt(i);
			b.append(c);
			permute(a, b, n);
			a.insert(i,c);
			b.setLength(b.length()-1);
		}
	}
    
For duplicates maintain hashset and print only if new.

### 9. Print all valid paranthesis (Backtracking)

	private static void pp(int o, int c, StringBuilder sb) {
		if(o==0 && c== 0) {
			System.out.println(sb.toString());
		}
		if(o>0) {
			sb.append('(');
			pp(o-1, c, sb);
			sb.setLength(sb.length() - 1);
		}
		if(c>o) {
			sb.append(')');
			pp(o, c-1, sb);
			sb.setLength(sb.length() - 1);
		}
	}

### 10. PaintFill (see CTCI)

### 11. Coins: Given a set of couns count number of ways to sum up the coins

	//f(n,i) = 1 // if(n==0)
	//f(n,i) = 0 // if(i>a.length)
	//f(n,i) = f(n-a[i],i) + f(n,i+1) // if n>=a[i]
	//f(n,i) = f(n,i+1) // otherwise
	
	private static int count(int[] a, int n) {
		int[] row1 = new int[n+1];
		int[] row2 = new int[n+1];
		for(int i = a.length-1; i>=0; i--) {
			for(int j=0; j<=n; j++) {
				if(j==0)
					row1[j] = 1;
				else if(j>=a[i])
					row1[j] = row1[j-a[i]] + row2[j];
				else
					row1[j] = row2[j];
			}
			for(int j=0; j<row1.length; j++)
				row2[j] = row1[j];
		}
		return row1[n];
	}

### 12. 8 Queens

### 13. Stack of boxes (See CTCI)

### 14. See CTCI
