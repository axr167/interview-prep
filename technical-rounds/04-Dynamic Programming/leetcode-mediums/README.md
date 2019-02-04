
## Overview:

Leetcode medium level dp problems.

## 5. Longest Palindromic Substring

### Question:
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

### Answer:
Let f(i,j) be the longest palindromic substring from s(i..j). If s(i..j) is pallindrome return s(i...j) otherwise return either f(i+1,j) or return f(i,j+1)

    /*
        f(i,j) = s.substring(i,j) // if(isP(s.substring(i,j)))
        f(i,j) = max( f(i,j-1), f(i+1,j) ) // otherwise
    */
    
    private String solve(String s) {
        String[] row1 = new String[s.length()+1];
        String[] row2 = new String[s.length()+1];
        
        for(int i=s.length()-1; i>=0;i--) {
            for(int j=i+1; j <= s.length(); j++) {
                if(isP(s.substring(i,j)))
                    row1[j] = s.substring(i,j);
                else
                    row1[j] = (row1[j-1].length() > row2[j].length())? row1[j-1]:row2[j];
            }
            for(int j=0; j<row1.length; j++)
                row2[j] = row1[j];
        }
        return row1[s.length()];
    }

## 322. Coin Change

### Question:
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

### Answer:
Let f(n,i) be number of coins needed for amount 'n' starting from index 'i' in the array. If n=0, return 0. If i=a.length n=max (since it is not possible), If we can use coin (n>a[i]) then either use the coin or dont min(f(n-a[i],i), f(n,i+1)) else if we cannot use the coin then try for next coin f(n, i+1).

    /*
        f(v,i) = max //if i >=a.length && v > 0
        f(v,i) = 0 // if v = 0
        f(v,i) = min(1+f(v-a[i],i), f(v,i+1)) // if(v-a[i] > 0)
        f(v,i) = f(v,i+1) // otherwise
    */
    
    private int f(int[] a, int v) {
        int[] row1 = new int[v+1];
        int[] row2 = new int[v+2];
        
        Arrays.fill(row2, Integer.MAX_VALUE-a.length);
        
        for(int i = a.length-1; i >= 0; i--) {
            for(int j=0; j<=v; j++) {
                if(j == 0)
                    row1[j] = 0;
                else {
                    if(j-a[i] >= 0)
                        row1[j] = Math.min(1+row1[j-a[i]], row2[j]);
                    else
                        row1[j] = row2[j];
                }
            }
            for(int j=0; j<row1.length; j++)
                row2[j] = row1[j];
        }
        return row1[v];
    }

## 139. Word Break

### Question:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

### Answer:
let f(i,j) return true if substring starting from i exists in dictionary. Here i represents start of string, j represents end of string. If i=s.length return true; if j=s.length return false; if s(i..j) exists in dictionary then either break the word or dont so return (f(j+1, j+1)||f(i,j+1)); if it does not exist in dictionary then increment j f(i,j+1).

    /*
        f(i,j) = true //if i,j == s.length   
        f(i,j) = false // if j == s.length
        f(i,j) = f(j+1, j+1) OR f(i, j+1) // if word in dict
        f(i,j) = f(i,j+1) // if word not in dict
    */
    
    private boolean f_o(String s, Set<String> set) {
        boolean[] col1 = new boolean[s.length()+1];
        boolean[] col2 = new boolean[s.length()+1];
        
        for(int i=0; i<col2.length-1;i++)
            col2[i] = false;
        col2[col2.length-1] = true;
        
        for(int j=s.length()-1; j>=0; j--) {
            for(int i=j; i>=0; i--) {
                String word = s.substring(i,j+1);
                if(set.contains(word))
                    col1[i] = (col2[j+1]||col2[i]);
                else
                    col1[i] = col2[i];
            }
            for(int i=0; i<col1.length; i++)
                col2[i] = col1[i];
        }
        return col1[0];
    }

## 91. Decode Ways

### Question:
A message containing letters from A-Z is being encoded to numbers using the following mapping: A:1, B:2, ..., Z:26. Given a non-empty string containing only digits, determine the total number of ways to decode it.

### Answer:
Let f(i) be number of ways to decode string starting from i. If i=string.length return 1. If s[i]=0 return 0. Otherwise if there are at least 2 digits after i and next 2 digits are between i and 26 return f(i+1)+f(i+2). Otherwise return f(i+1).

    /*
        f(i) = 1 //if i=s.length
        f(i) = 0 //if s[i] = 0
        f(i) = f(i+1) + f(i+2) //if s(i..i+1) is valid
        f(i) = d(i+1) otherwise
    */
    
    private int num(String s) {
        int i1 = 1;
        int i2 = 1;
        int i0 = 0;
        for(int i=s.length()-1; i>=0; i--) {
            if(s.charAt(i) == '0')
                i0 = 0;
            else if(s.length()-i>=2 && valid(s.substring(i,i+2)))
                i0 = i1+i2;
            else
                i0 = i1;
            i2 = i1;
            i1 = i0;
        }
        return i0;
    } 

## 152. Maximum Product Subarray

### Question:
Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

### Answer:
Let f(p,i) be max product subarray until index i. If i=a.length return p; Otherwise 

