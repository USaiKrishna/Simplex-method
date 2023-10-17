# Simplex-method
// Maximize x + 3y subject to the constraints:
    // 3x + 6y <= 8
    // 5x + 2y <= 10
    // x, y >= 0
#include <stdio.h>

void printTable(float table[3][5], int m, int n) {
    int i, j;
    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++) {
            printf("%.2f\t", table[i][j]);
        }
        printf("\n");
    }
    printf("\n");
}

void simplex(float table[3][5], int m, int n) {
    int i, j;
    while (1) {
        float minVal = table[0][1];
        int pivotColumn = 1;
        for (j = 1; j < n - 1; j++) {
            if (table[0][j] < minVal) {
                minVal = table[0][j];
                pivotColumn = j;
            }
        }
        
        if (minVal >= 0) {
            printf("Optimal solution found:\n");
            printf("Z = %.2f\n", table[0][n - 1]);
            printf("x = %.2f\n", table[1][n - 1]);
            printf("y = %.2f\n", table[2][n - 1]);
            break;
        }
        
        float minRatio = -1;
        int pivotRow = -1;
        for (i = 1; i < m; i++) {
            if (table[i][pivotColumn] > 0) {
                float ratio = table[i][n - 1] / table[i][pivotColumn];
                if (minRatio == -1 || ratio < minRatio) {
                    minRatio = ratio;
                    pivotRow = i;
                }
            }
        }
        
        float pivotElement = table[pivotRow][pivotColumn];
        for (j = 0; j < n; j++) {
            table[pivotRow][j] /= pivotElement;
        }
        for (i = 0; i < m; i++) {
            if (i != pivotRow) {
                float factor = table[i][pivotColumn];
                for (j = 0; j < n; j++) {
                    table[i][j] -= factor * table[pivotRow][j];
                }
            }
        }
    }
}

int main() {
    float table[3][5] = {
        {1, -1, -3, 0, 0},
        {0, 3, 6, 1, 8},
        {0, 5, 2, 0, 10}
    };
    
    int m = 3;
    int n = 5;
    
    simplex(table, m, n);
    
    return 0;
}
