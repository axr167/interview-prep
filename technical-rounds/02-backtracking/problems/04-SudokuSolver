
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
