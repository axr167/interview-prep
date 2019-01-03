
## Paradigm:

1. Same as recursion : Manually change problem by performing first step
2. Check if we have reached a valid result
    - If yes, print/store result
    - Else call recursive function for the remaining problem
3. Undo the changes made in step 1 after performing recursion

## Functions:

- **boolean isSafe(row, col, board)**: Return true if it is safe to place a queen at position (row, col) on the board
- **void saveBoard(board, list)**: Saves board to list
- **void nQueenBacktrack(row, board, list)**: Backtracking algorithm that solves the problem for a given board

## Backtracking algorithm

      private void nQueenBacktrack(int row, char[][] board, List<List<String>> list) {
          for(int i=0; i< board.length; i++) {
              if(isSafe(row, i, board)) {
                  board[row][i] = 'Q';
                  if(row == board.length-1)
                      saveBoard(board, list);
                  else
                      nQueenBacktrack(row+1, board, list);
                  board[row][i] = '.';
              }
          }
      }

- Start at 0,0
- Step 1 of paradigm: If it is safe to place queen, place it
- Step 2 of paradigm: Have we reached result? 
    - If yes save to list
    - Else call recursive function for remaining board
- Step 3 of paradigm: Remove previously placed queen

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
            for(int i=0; i< board.length; i++) {
                if(isSafe(row, i, board)) {
                    board[row][i] = 'Q';
                    if(row == board.length-1)
                        saveBoard(board, list);
                    else
                        nQueenBacktrack(row+1, board, list);
                    board[row][i] = '.';
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
