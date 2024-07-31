# Tic-Toe-Game
The game is played on a 3x3 grid, with two players: X (user) and O (computer). The game starts with an empty grid, and players take turns marking a square. The first player to get three in a row (horizontally, vertically, or diagonally) wins the game. If all squares are filled and no player has won, the game is a draw.
import random

# The game board
board = [' ' for _ in range(9)]

# Function to draw the game board
def draw_board():
    row1 = '| {} | {} | {} |'.format(board[0], board[1], board[2])
    row2 = '| {} | {} | {} |'.format(board[3], board[4], board[5])
    row3 = '| {} | {} | {} |'.format(board[6], board[7], board[8])

    print()
    print(row1)
    print(row2)
    print(row3)
    print()

# Function to handle user move
def user_move():
    run = True
    while run:
        move = input('Please select a position to place an \'X\' (1-9): ')
        try:
            move = int(move)
            if move > 0 and move < 10:
                if space_is_free(move):
                    run = False
                    insert_letter('X', move)
                else:
                    print('Sorry, this space is occupied!')
            else:
                print('Please type a number within the range!')
        except:
            print('Please type a number!')

# Function to handle computer move
def comp_move():
    best_score = float('-inf')
    best_move = 0
    for key in range(9):
        if board[key] == ' ':
            board[key] = 'O'
            score = minimax(board, 0, False)
            board[key] = ' '
            if score > best_score:
                best_score = score
                best_move = key
    insert_letter('O', best_move + 1)
    return

# Function to check if a space is free
def space_is_free(pos):
    return board[pos - 1] == ' '

# Function to insert a letter in the board
def insert_letter(letter, pos):
    board[pos - 1] = letter

# Function to check if the board is full
def is_board_full(board):
    return board.count(' ') == 0

# Function to check for a win
def is_winner(b, l):
    winning_combos = [(0, 1, 2), (3, 4, 5), (6, 7, 8), (0, 3, 6), (1, 4, 7), (2, 5, 8), (0, 4, 8), (2, 4, 6)]
    for combo in winning_combos:
        if b[combo[0]] == b[combo[1]] == b[combo[2]] == l:
            return True
    return False

# Minimax function
def minimax(board, depth, is_maximizing):
    if is_winner(board, 'O'):
        return 10
    elif is_winner(board, 'X'):
        return -10
    elif is_board_full(board):
        return 0

    if is_maximizing:
        best_score = float('-inf')
        for key in range(9):
            if board[key] == ' ':
                board[key] = 'O'
                score = minimax(board, depth + 1, False)
                board[key] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = float('inf')
        for key in range(9):
            if board[key] == ' ':
                board[key] = 'X'
                score = minimax(board, depth + 1, True)
                board[key] = ' '
                best_score = min(score, best_score)
        return best_score

# Main game loop
def play_game():
    draw_board()
    while not(is_board_full(board)):
        if not(is_winner(board, 'O')):
            user_move()
            draw_board()
        else:
            print('Sorry, O\'s won this time!')
            break

        if not(is_winner(board, 'X')):
            comp_move()
            draw_board()
        else
