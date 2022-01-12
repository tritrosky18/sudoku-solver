# sudoku-solver
This is the sudoku solver I made following a tutorial.


def find_next_empty(puzzle):
    #finds the next row, col on the puzzle that is not filled ---> -1
    #return row, col tuple (or(None,None)  if there is none)
    
    for r in range(9):
        for c in range(9):
            if puzzle[r][c] == -1:
                return r,c
            
    return None,None # if no spaces in the puzzle are empty (-1)


def is_valid(puzzle, guess, row, col):
    #figures out whether the guess at row,col is a valid guess
    #returns True if valid, False if not
    
    #let's start with the row
    row_vals = puzzle[row]
    if guess in row_vals:
        return False
    
    #now columns
    #col_vals = []
    #for i in range(9):
    #    col_vals.append(puzzle[i][col])
    col_vals = [puzzle[i][col] for i in range(9)]
    if guess in col_vals:
        return False
    
    #3x3 matrix. Figure out in 3x3 grid where we are. Start with row
    #index then col index. Iterate over the 3 values in row/col
    row_start = (row // 3) *3
    col_start = (col // 3) * 3
    
    for r in range(row_start, row_start + 3):
        for c in range(col_start, col_start + 3):
            if puzzle [r][c] == guess:
                return False

    #if we get here, these checks pass
    return True





def solve_sudoku(puzzle):
    # solve sudoku using back tracking technique
    # our puzzle is a list of lists, 
    #return whether a solution exists
    #mutates puzzles to be solution if one exists
    
    #step 1: choose where on the puzzle we can go
    row,col = find_next_empty(puzzle)
    
    #step 1.1: if there's nowhere left, then we're done because we only allowed
    #valid inputs
    if row is None:
        return True #solved puzzle
    
    #step 2: if theere is a place to put our number, then make a
    #guess between 1 and 9, try all until it works
    for guess in range(1,10):
        #step 3: Check if a valid guess
        if is_valid(puzzle, guess, row, col):
            #step 3.1: if this is valid, then place that guess on puzzle
            puzzle[row][col] = guess
            #step 4: now recursively call function
            if solve_sudoku(puzzle):
                return True
            
        #step 5: if not valid OR if our guess dose not solve the puzzle,
        #then we need to backtrack and try a new number
        puzzle[row][col] = -1  #reset guess
        
    #step 6: if none of the number that we try work, then this puzzle
    #is unsolvable
    return False


if __name__ == '__main__':
    example_board = [
        [3,9,-1,   -1,5,-1,   -1,-1,-1],
        [-1,-1,-1,   2,-1,-1,   -1,-1,5],
        [-1,-1,-1,   7,1,9,   -1,8,-1],
        
        [-1,5,-1,   -1,6,8,   -1,-1,-1],
        [2,-1,6,   -1,-1,3,   -1,-1,-1],
        [-1,-1,-1,   -1,-1,-1,   -1,-1,4],
        
        [5,-1,-1,   -1,-1,-1,   -1,-1,-1],
        [6,7,-1,   1,-1,5,   -1,4,-1],
        [1,-1,9,   -1,-1,-1,   2,-1,-1]
    ]
    print(solve_sudoku(example_board))
    print(example_board)
        
