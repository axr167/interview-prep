
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

