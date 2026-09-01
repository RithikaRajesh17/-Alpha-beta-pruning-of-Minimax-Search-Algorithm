<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>Name:  Rithika R     </h3>
<h3>Register Number/Staff Id:  212224240136    </h3>
<H3>Aim:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h1>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h1>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h1>IMPLEMENTATION</h1>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h1>The Minimax algorithm</h1>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h1>Alpha-Beta pruning</h1>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
<h2>Sample Input and Output:</h2>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)


<h3>Program</h3>

```py
# Alpha-Beta Pruning for Tic-Tac-Toe

def print_board(board):
    print()
    for row in board:
        print(" | ".join(row))
        print("--+---+--")
    print()


def check_winner(board):
    # Rows
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] != ' ':
            return board[i][0]

    # Columns
    for i in range(3):
        if board[0][i] == board[1][i] == board[2][i] != ' ':
            return board[0][i]

    # Diagonals
    if board[0][0] == board[1][1] == board[2][2] != ' ':
        return board[0][0]

    if board[0][2] == board[1][1] == board[2][0] != ' ':
        return board[0][2]

    return None


def is_full(board):
    for row in board:
        if ' ' in row:
            return False
    return True


def alpha_beta(board, depth, alpha, beta, maximizing):
    winner = check_winner(board)

    if winner == 'O':
        return 10 - depth

    if winner == 'X':
        return depth - 10

    if is_full(board):
        return 0

    if maximizing:
        best_score = -1000

        for i in range(3):
            for j in range(3):
                if board[i][j] == ' ':
                    board[i][j] = 'O'

                    score = alpha_beta(
                        board, depth + 1, alpha, beta, False
                    )

                    board[i][j] = ' '

                    best_score = max(best_score, score)
                    alpha = max(alpha, best_score)

                    # Alpha-Beta pruning
                    if beta <= alpha:
                        break

            if beta <= alpha:
                break

        return best_score

    else:
        best_score = 1000

        for i in range(3):
            for j in range(3):
                if board[i][j] == ' ':
                    board[i][j] = 'X'

                    score = alpha_beta(
                        board, depth + 1, alpha, beta, True
                    )

                    board[i][j] = ' '

                    best_score = min(best_score, score)
                    beta = min(beta, best_score)

                    # Alpha-Beta pruning
                    if beta <= alpha:
                        break

            if beta <= alpha:
                break

        return best_score


def best_move(board):
    best_score = -1000
    move = None

    for i in range(3):
        for j in range(3):
            if board[i][j] == ' ':
                board[i][j] = 'O'

                score = alpha_beta(
                    board, 0, -1000, 1000, False
                )

                board[i][j] = ' '

                if score > best_score:
                    best_score = score
                    move = (i, j)

    return move


# Initial board
board = [
    [' ', ' ', ' '],
    [' ', ' ', ' '],
    [' ', ' ', ' ']
]

print("TIC-TAC-TOE")
print("You are X")
print("Computer is O")
print("Enter row and column from 1 to 3")

while True:

    print_board(board)

    # Player move
    row, col = map(
        int, input("Enter your move (row col): ").split()
    )

    row -= 1
    col -= 1

    if row < 0 or row > 2 or col < 0 or col > 2:
        print("Invalid position!")
        continue

    if board[row][col] != ' ':
        print("Position already occupied!")
        continue

    board[row][col] = 'X'

    # Check player win
    if check_winner(board) == 'X':
        print_board(board)
        print("You win!")
        break

    # Check draw
    if is_full(board):
        print_board(board)
        print("Draw!")
        break

    # Computer move using Alpha-Beta pruning
    move = best_move(board)

    board[move[0]][move[1]] = 'O'

    print(
        "Computer played:",
        move[0] + 1,
        move[1] + 1
    )

    # Check computer win
    if check_winner(board) == 'O':
        print_board(board)
        print("Computer wins!")
        break

    # Check draw
    if is_full(board):
        print_board(board)
        print("Draw!")
        break
```

<h3>Output</h3>
<img width="638" height="637" alt="image" src="https://github.com/user-attachments/assets/18305fbc-9134-4d25-b710-004ee7e60556" />


<h3>Result</h3>
Thus, Alpha-Beta pruning of the Minimax Search Algorithm
