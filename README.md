# TIC-TAC-TOE
# TIC TAC TOE Game using Data Structure Algorithm
#include <stdio.h>
#include <stdlib.h>

#define QUEUE_SIZE 2  // We need a queue for two players

// Function declarations
void printBoard(char b[3][3]);
void GameW(char b[3][3], char turn);
void resetBoard(char b[3][3]);
void Roption(char b[3][3]);

// Queue structure for managing player turns
typedef struct {
    char turns[QUEUE_SIZE]; // Store the turns (X, O)
    int front, rear;
} Queue;

// Queue functions
void enqueue(Queue *q, char player);
char dequeue(Queue *q);
int isQueueEmpty(Queue *q);

// Main function
int main() {
    int op;
    char b[3][3] = {{'1', '2', '3'}, {'4', '5', '6'}, {'7', '8', '9'}}; // Game board
    Queue q = {{'X', 'O'}, 0, 1}; // Initialize queue with 'X' first and 'O' second
    int i, j;
    char turn;

    printBoard(b);

    // Game loop
    for (int c = 0; c < 9; c++) {
        turn = dequeue(&q);  // Get current player's turn
        int T, row, col;
        printf("Enter the position for %c: ", turn);
        scanf("%d", &T);

        // Map input to board position
        switch (T) {
            case 1: row = 0; col = 0; break;
            case 2: row = 0; col = 1; break;
            case 3: row = 0; col = 2; break;
            case 4: row = 1; col = 0; break;
            case 5: row = 1; col = 1; break;
            case 6: row = 1; col = 2; break;
            case 7: row = 2; col = 0; break;
            case 8: row = 2; col = 1; break;
            case 9: row = 2; col = 2; break;
            default:
                c--;
                printf("\nInvalid position\n");
                continue;
        }

        // Check if the selected position is valid
        if (b[row][col] != 'X' && b[row][col] != 'O') {
            b[row][col] = turn;
            printBoard(b);

            // Check if the game has a winner
            GameW(b, turn);

            // Enqueue the next player's turn
            enqueue(&q, (turn == 'X') ? 'O' : 'X');
        } else {
            printf("Box is already filled, try again.\n");
            c--;  // Stay on the same turn if the position is invalid
        }
    }
}

// Function to print the game board
void printBoard(char b[3][3]) {
    system("cls");  // Clear screen
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("  %c", b[i][j]);
            if (j < 2) {
                printf("  | ");
            }
        }
        printf("\n");
        if (i < 2) {
            printf("-----|------|------\n");
        }
    }
}

// Function to check for a winner
void GameW(char b[3][3], char turn) {
    int i, j, filled = 1;
    
    // Check for win conditions
    for (i = 0; i < 3; i++) {
        if ((b[i][0] == b[i][1] && b[i][1] == b[i][2]) || (b[0][i] == b[1][i] && b[1][i] == b[2][i])) {
            printf("Player %c wins!\n", (turn == 'X') ? 'O' : 'X');
            Roption(b);
        }
    }
    if ((b[0][0] == b[1][1] && b[0][0] == b[2][2]) || (b[0][2] == b[1][1] && b[0][2] == b[2][0])) {
        printf("Player %c wins!\n", (turn == 'X') ? 'O' : 'X');
        Roption(b);
    }

    // Check for draw condition
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            if (b[i][j] != 'X' && b[i][j] != 'O') {
                filled = 0;  // Found an empty space
                break;
            }
        }
        if (!filled) {
            break;
        }
    }

    if (filled) {
        printf("It's a draw!\n");
        Roption(b);
    }
}

// Function to reset the board
void resetBoard(char b[3][3]) {
    int i, j;
    char initial = '1';
    for (i = 0; i < 3; i++) {
        for (j = 0; j < 3; j++) {
            b[i][j] = initial++;  // Reset each cell to its original number
        }
    }
}

// Function to provide reset or exit option
void Roption(char b[3][3]) {
    int re;
    printf("Do you want to reset the game? 1.Yes or 2.No & exit: ");
    scanf("%d", &re);
    if (re == 1) {
        resetBoard(b);
        printBoard(b);
        main();  // Restart the game
    } else {
        exit(0);  // Exit the game
    }
}

// Queue functions
void enqueue(Queue *q, char player) {
    q->turns[q->rear] = player;
    q->rear = (q->rear + 1) % QUEUE_SIZE;
}

char dequeue(Queue *q) {
    char player = q->turns[q->front];
    q->front = (q->front + 1) % QUEUE_SIZE;
    return player;
}

int isQueueEmpty(Queue *q) {
    return q->front == q->rear;
}

