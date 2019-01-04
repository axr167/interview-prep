
# Backtracking/Exhaustive search (In progress)


## Paradigm:

- Iterative exhaustive search is straightforward. Have a bunch of nested loops and loop through all possible values. Filter out all unneeded values.
- Recursive backtracking is similar to recursion but with a minor difference: Base case is result so instead of returning, save/print result
- In induction step after performing recursion undo changes to reset problem to original state

Paradigm is:

- If base case Save/print output
- Else do induction step - This is done a finite 'n' number of times which is equal to the search space
    - Do first step reducing problem size
    - Recurse for subproblem
    - Undo all changes
      

## When to use this:

Use backtracking/exhaustive search when you either have to go back or when the problem may have more than 1 result and you want all of them.

## Problems:

1. N Queens (Leetcode 51)
2. Permutations of an array (Leetcode 46)
3. Word Search (Leetcode 79)
4. Sudoku solver (Leetcode 37)
5. Combination Sum (Leetcode 39)


# 1. N Queens

## Paradigm:

1. If base case, save/print result
2. Else recurse and backtrack
    - Do first step to reduce problem size
    - Recurse
    - Undo changes and proceed

## Functions:

- **boolean isSafe(row, col, board)**: Return true if it is safe to place a queen at position (row, col) on the board
- **void saveBoard(board, list)**: Saves board to list
- **void nQueenBacktrack(row, board, list)**: Backtracking algorithm that solves the problem for a given board

## Backtracking algorithm

    private void nQueenBacktrack(int row, char[][] board, List<List<String>> list) {
        if(row == board.length)                         // BASE CASE
            saveBoard(board, list);
        else {                                          // INDUCTION STEP
            for(int i=0; i< board.length; i++) {
                if(isSafe(row, i, board)) {
                    
                    // Choose queen
                    board[row][i] = 'Q';
                    
                    // Recurse
                    nQueenBacktrack(row+1, board, list);
                    
                    // Undo Changes
                    board[row][i] = '.';
                }
            }
        }
    }

- If we have filled last row, we have a result. Save board to list
- Else for each column in board to check if it is safe. If safe do:
      - Add queen to position
      - Recurse for remaining board
      - Undo changes

## Full code

Full code as follows:

    class Solution {

        private void saveBoard(char[][] board, List<List<String>> list) {
            List<String> result = new ArrayList<>();
            for(int i = 0; i < board.length; i++) {
                String s = new String(board[i]);
                result.add(s);
            }
            list.add(result);
        }

        // Checks if safe to place queen at position row, col
        private boolean isSafe(int row, int col, char[][] board) {
            //Check if row is safe
            for(int i=0; i<board.length; i++) {
                if(board[i][col] == 'Q')
                    return false;
            }
            //Check if col is safe
            for(int i=0; i<board.length; i++) {
                if(board[row][i] == 'Q')
                    return false;
            }
            //Check if diagonals are safe
            int d1 = row - col;
            int d2 = row + col;
            for(int i=0; i<board.length; i++) {
                if(i-d1 >= 0 && i-d1 < board.length) {
                    if(board[i][i-d1] == 'Q')
                        return false;
                }
                if(d2-i >= 0 && d2-i < board.length) {
                    if(board[i][d2-i] == 'Q')
                        return false;
                }
            }
            return true;
        }

        private void nQueenBacktrack(int row, char[][] board, List<List<String>> list) {
            if(row == board.length)                         // BASE CASE
                saveBoard(board, list);
            else {                                          // INDUCTION STEP
                for(int i=0; i< board.length; i++) {
                    if(isSafe(row, i, board)) {

                        // Choose queen
                        board[row][i] = 'Q';

                        // Recurse
                        nQueenBacktrack(row+1, board, list);

                        // Undo Changes
                        board[row][i] = '.';
                    }
                }
            }
        }

        public List<List<String>> solveNQueens(int n) {

            List<List<String>> list = new ArrayList<>();
            char[][] board = new char[n][n];
            for(char[] row: board)
                Arrays.fill(row, '.');
            nQueenBacktrack(0, board, list);

            return list;
        }
    }
