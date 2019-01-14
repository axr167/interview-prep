
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

Use backtracking/exhaustive search when you either have to go through entire search space.

## Problems:

1. N Queens (Leetcode 51)
2. Permutations of an array (Leetcode 46)
3. Sudoku solver (Leetcode 37)
4. Combination Sum (Leetcode 39)


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

# 2. Permutations of an array


## Paradigm:

1. If base case, save/print result
2. Else recurse and backtrack
    - Do first step to reduce problem size
    - Recurse
    - Undo changes and proceed

## Functions:

- **void permute(list, chosen, res)**: Backtracking algorithm that gets permutations of list and stores them in result

## Backtracking algorithm

      private void permute(List<Integer> list, List<Integer> chosen, List<List<Integer>> res) {
          if(list.isEmpty()) {                          // BASE CASE
              res.add(new ArrayList<Integer>(chosen));
          } else {                                      // INDUCTION STEP
              for(int i=0; i< list.size(); i++) {

                  // choose element from list and add to chosen
                  int x = list.get(i);
                  chosen.add(x);
                  list.remove(i);

                  // Recurse
                  permute(list, chosen, res);

                  // Undo changes
                  list.add(i, x);
                  chosen.remove(chosen.size()-1);
              }
          }
      }

- If list is empty, chosen contains a possible result. Add it to list of results
- Sequentially choose elements from list and add them to chosen
    - Permute the remaining elements
    - Unchoose element from list by adding it back to the list and removing from chosen.

## Full code

Full code as follows:

      class Solution {

          private void permute(List<Integer> list, List<Integer> chosen, List<List<Integer>> res) {
              if(list.isEmpty()) {
                  res.add(new ArrayList<Integer>(chosen));
              } else {
                  for(int i=0; i< list.size(); i++) {

                      // choose element from list and add to chosen
                      int x = list.get(i);
                      chosen.add(x);
                      list.remove(i);

                      // Recurse
                      permute(list, chosen, res);

                      // Undo changes
                      list.add(i, x);
                      chosen.remove(chosen.size()-1);
                  }
              }
          }

          public List<List<Integer>> permute(int[] nums) {

              List<List<Integer>> res = new ArrayList<>();
              List <Integer> chosen = new ArrayList<>();
              List <Integer> list = new ArrayList<>();

              for(int i: nums){
                  list.add(i);
              }

              permute(list, chosen, res);
              return res;

          }
      }


# 3. Sudoku Solver (In Progress)


Note: This will be edited later


    class Solution {

        private boolean isSafe(char val, int row, int col, char[][] board) {

            // Check if row is safe
            for(int i = 0; i< board.length; i++) {
                if(board[i][col] == val)
                    return false;
            }

            // Check if col is safe
            for(int i = 0; i< board.length; i++) {
                if(board[row][i] == val)
                    return false;
            }

            // check if grid is safe
            int row_end = (row/3)+1;
            int col_end = (col/3)+1;

            row_end = row_end*3;
            col_end = col_end*3;

            for(int i = row_end-3; i<row_end; i++) {
                for (int j = col_end-3; j < col_end; j++) {
                    if(board[i][j] == val)
                        return false;
                }
            }

            return true;
        }

        private boolean solve(char[][] board) {
            for(int row = 0; row < board.length; row++) {
                for(int col = 0; col < board[0].length; col++) {
                    if(board[row][col] == '.') {
                        for(char val = '1' ; val<= '9' ; val++) {
                            if(isSafe(val, row, col, board)) {

                                // Add number to board
                                board[row][col] = val;

                                // Recurse
                                if(solve(board))
                                    return true;

                                // Undo Changes
                                board[row][col] = '.';

                            }
                        } 
                        return false;
                    }
                }
            }
            return true;
        }


        public void solveSudoku(char[][] board) {
            solve(board);
        }
    }

# Combination sum

    class Solution {

        private void solve(int[] a, int target, List<Integer> chosen, List<List<Integer>> res, int start) {
            if(target == 0) {
                res.add(new ArrayList<Integer>(chosen));
            } else if(target > 0) {
                for(int i = start; i<a.length; i++) {
                    chosen.add(a[i]);

                    solve(a, (target-a[i]), chosen, res, i);

                    chosen.remove(chosen.size()-1);
                }
            }
        }

        public List<List<Integer>> combinationSum(int[] candidates, int target) {

            List<List<Integer>> res = new ArrayList<>();
            List<Integer> chosen = new ArrayList<>();
            Arrays.sort(candidates);

            solve(candidates, target, chosen, res, 0);
            return res;

        }
    }


