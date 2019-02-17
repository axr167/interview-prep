
### 1. Implement algorithm to determine if string has all unique characters

    // O(n) space, O(n) time
    private static boolean check(String s) {
      boolean[] chars = new boolean[128];
      int count = 0;
      for(int i=0; i<s.length(); i++) {
        int x = s.charAt(i);
        if(chars[x] == false){
          count++;
          chars[x] = true;
        }
      }
      System.out.println(count);
      if(count==s.length())
        return true;
      else
        return false;
    }


If we cannot use additional data structures we can either

- Compare each char to every other char and see if it exists
- Sort string and check for adjacent neighbours

### 2. Check if 1 string is permutation of the other

    private static boolean check(String s1, String s2) {
      int[] a = new int[128];
      for(int i=0; i<s1.length(); i++) {
        int x = (int) s1.charAt(i);
        a[x]++;
      }
      for(int i=0; i<s2.length(); i++) {
        int x = (int) s2.charAt(i);
        a[x]--;
      }
      for(int i=0; i<a.length; i++) {
        if(a[i] != 0)
          return false;
      }
      return true;
    }

### 3. URLify

### 4. Check if string is permutation of a palindrome

    private static boolean check(String s) {
      Map<Character, Integer> map = new HashMap<>();
      int count = 0;
      for(int i=0; i<s.length(); i++) {
        if(!map.containsKey(s.charAt(i))) {
          count++;
          map.put(s.charAt(i), 1);
        } else {
          int x = map.get(s.charAt(i))+1;
          if(x%2==0)
            count--;
          else
            count++;
          map.put(s.charAt(i), x);
        }
      }
      if(count==0||count==1)
        return true;
      return false;
    }

### 5. Check if string 1 is one edit away from string 2. An edit is insertion, deletion or replacement.

	  private static boolean check_replace(String s1, String s2) {
      int count = 0;
      for(int i=0; i<s1.length(); i++) {
        if(s1.charAt(i)!=s2.charAt(i)) {
          count++;
          if(count >=2)
            return false;
        }
      }
      return true;
    }
    private static boolean check_insert(String s1, String s2) {
      int x=0; int y=0; int count = 0;
      while(x != s1.length()) {
        if(s1.charAt(x) != s2.charAt(y)) {
          count++;
          if(count>=2)
            return false;
          x++;
        } else {
          x++;
          y++;
        }
      }
      return true;
    }

    private static boolean check(String s1, String s2) {
      if(s1.length()-s2.length()>1)
        return false;
      else if(s1.length()-s2.length() == 1)
        return check_insert(s1, s2);
      else
        return check_replace(s1, s2);
    }

### 6. String compression - change aabbbccaa to a2b3c2a2

    private static String compress(String s) {
      StringBuilder sb = new StringBuilder();
      sb.append(s.charAt(0));
      char prev = s.charAt(0);
      int count = 1;
      for(int i=1; i<s.length(); i++) {
        if(s.charAt(i) == prev)
          count++;
        else {
          sb.append(count);
          count = 1;
          sb.append(s.charAt(i));
          prev = s.charAt(i);
        }
      }
      sb.append(count);
      return sb.toString();
    }
    
### 7. Rotate matrix

Logic: First do transpose -> invert either left-right or up-down
