
## Types of problems encountered:

**Slow pointer/Fast pointer approach**
- Here initialize slow and fast pointers
- Ends when either slow = fast (loop in list etc) or when fast = end (remove duplicates)
- i can move slower or as fast as j

**Start/End approach**

- Initialize p1 to start of array
- Initialize p2 to end of the array
- Either increment p1 or decrement p2 to maximize/minimize something and keep doing so until p1>=p2
- Examples include trapping rainwater and 2/3/4-sum.

**Slow/Fast pointer with memory**
- This is used in problems where you must find a subarray/substring/any continuous block such that the block satisfies certain conditions.
- Have slow/fast pointer and a hashmap/hashset as supporting cache
- Increment fast pointer adding element to cache until the condition is violated.
- When condition is violated, increment slow pointer until condition is satisfied again removing things from cache as i is incremented
- Do until fast pointer reaches end.

### 1. Put array duplicates at end of sorted array

Slow pointer/fast pointer approach.

If i == j then increment j.
If i is not equal to j then increment i and replace i,j values

    public int removeDuplicates(int[] a) {
        if(a.length == 0)
            return 0;
        int i = 0; int j = 1;
        while(j!=a.length) {
            if(a[i] == a[j])
                j++;
            else {
                i++;
                a[i] = a[j];
                j++;
            }
        }
        return i+1;
    }

### 2. Container with most water

Start/end approach. Have 2 pointers at each end and check height of both pointers. If h(p1) > h(p2) decrement p2 otherwise increment p1.

    public int maxArea(int[] a) {
        int p1 = 0;
        int p2 = a.length-1;
        int max_vol = 0;
        while(p1 < p2) {
            int vol = Math.min(a[p1],a[p2])*(p2-p1);
            if(vol > max_vol)
                max_vol = vol;
            if(a[p1] > a[p2])
                p2--;
            else
                p1++;
        }
        return max_vol;
    }

### 3. Longest substring without repeating characters

Slow/fast pointer approach with cache. Condition is substring should not have repeating characters. Store all characters p2 encounters in a hashset and if condition is violated, increment p1.

    public int lengthOfLongestSubstring(String s) {
        
        if(s.length() <= 1)
            return s.length();
        
        Set<Character> set = new HashSet<>();
        int p1 = 0;
        set.add(s.charAt(p1));
        int p2 = 1;
        int best = 1;
        while(p2 < s.length()) {
            if(!set.contains(s.charAt(p2))) {
                set.add(s.charAt(p2));
                p2++;
                if(set.size() > best)
                    best = set.size();
            } else {
                while(set.contains(s.charAt(p2))) {
                    set.remove(s.charAt(p1));
                    p1++;
                }
                set.add(s.charAt(p2));
                p2++;
                if(set.size() > best)
                    best = set.size();
            }
        }
        return best;  
    }
    
### 4. Longest substring with at most 2 distinct chars 
 
Slow/fast pointers with cache. Maintain a hashmap with char counts. If map.size() > 2 then increment p1 and decrement map.get(p1) value by 1. if map.get(p1) == 0 then remove char p1 from map.
 
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if(s.length() <=2)
            return s.length();
        Map<Character, Integer> map = new HashMap<>();
        int p1 = 0;
        int p2 = 1;
        int best = 1;
        int current = 1;
        map.put(s.charAt(p1), 1);
        while(p2 < s.length()) {
            char c = s.charAt(p2);
            if(map.containsKey(c)) {
                current++;
                map.put(c, map.get(c)+1);
                p2++;
                if(current > best)
                    best = current;
            } else if (!map.containsKey(c) && map.size() < 2) {
                map.put(c, 1);
                current++;
                p2++;
                if(current > best)
                    best = current;
            } else {
                while(map.size() > 1) {
                    char x = s.charAt(p1);
                    map.put(x, map.get(x)-1);
                    p1++;
                    current--;
                    if(map.get(x) == 0)
                        map.remove(x);
                }
            }
        }
        return best;
    } 

### 5. Minimum Window Substring

    public String minWindow(String s, String t) {
        if(t.length() > s.length())
            return "";
        
        Set<Character> set = new HashSet<>();
		for(int i=0; i<t.length(); i++)
			set.add(t.charAt(i));
		Map<Character, Integer> map = new HashMap<>();
		int p1 = 0;
		int p2 = 0;
		int best = Integer.MAX_VALUE;
		String res = "";
		
		while(p2 < s.length()) {
			char c = s.charAt(p2);
			if(!set.contains(c))
				p2++;
			else {
				if(map.containsKey(c)) {
					map.put(c, map.get(c)+1);
					p2++;
				} else {
					map.put(c, 1);
					p2++;
					if(map.size() == set.size()) {
						if(p2-p1 < best) {
							best = p2-p1;
							res = s.substring(p1, p2);
						}
						while(map.size() == set.size()) {
							if(p2-p1 < best) {
								best = p2-p1;
								res = s.substring(p1, p2);
							}
							char x = s.charAt(p1);
							if(!set.contains(x))
								p1++;
							else {
								map.put(x, map.get(x)-1);
								if(map.get(x) == 0)
									map.remove(x);
								p1++;
							}
						}
					}
				}
			}
		}
		return res;
    }
