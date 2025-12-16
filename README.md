#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

int main() {
    int M, N;
    
    // Ввод размеров матрицы
    printf("Введите количество строк M: ");
    scanf("%d", &M);
    printf("Введите количество столбцов N: ");
    scanf("%d", &N);
    
    // Проверка корректности размеров
    if (M <= 0 || N <= 0) {
        printf("Ошибка: размеры матрицы должны быть положительными\n");
        return 1;
    }
    
    // Выделение памяти для матрицы
    int **matrix = (int**)malloc(M * sizeof(int*));
    if (matrix == NULL) {
        printf("Ошибка выделения памяти\n");
        return 1;
    }
    for (int i = 0; i < M; i++) {
        matrix[i] = (int*)malloc(N * sizeof(int));
        if (matrix[i] == NULL) {
            printf("Ошибка выделения памяти\n");
            return 1;
        }
    }
    
    // Ввод элементов матрицы
    printf("Введите элементы матрицы %dx%d:\n", M, N);
    for (int i = 0; i < M; i++) {
        for (int j = 0; j < N; j++) {
            scanf("%d", &matrix[i][j]);
        }
    }
    
    // Вывод матрицы для наглядности
    printf("\nВведенная матрица:\n");
    for (int i = 0; i < M; i++) {
        for (int j = 0; j < N; j++) {
            printf("%4d", matrix[i][j]);
        }
        printf("\n");
    }
    
    int max_duplicates = 0;  // Максимальное количество одинаковых элементов в столбце
    int first_max_col = -1;  // Номер первого столбца с максимальным количеством дубликатов
    
    // Анализ каждого столбца
    for (int j = 0; j < N; j++) {
        int current_max_duplicates = 1;  // Минимум 1 повторение (сам элемент)
        
        // Подсчет максимального количества одинаковых элементов в столбце j
        for (int i = 0; i < M; i++) {
            int count = 1;  // Считаем текущий элемент
            
            // Сравниваем элемент matrix[i][j] со всеми последующими в столбце
            for (int k = i + 1; k < M; k++) {
                if (matrix[i][j] == matrix[k][j]) {
                    count++;
                }
            }
            
            // Обновляем максимальное количество для этого столбца
            if (count > current_max_duplicates) {
                current_max_duplicates = count;
            }
        }
        
        // Проверяем, является ли этот столбец новым максимумом
        if (current_max_duplicates > max_duplicates) {
            max_duplicates = current_max_duplicates;
            first_max_col = j;  // Запоминаем номер первого такого столбца
        }
    }
    
    // Вывод результата
    printf("\nРезультат:\n");
    if (first_max_col != -1) {
        printf("Первый столбец с максимальным количеством одинаковых элементов: %d\n", first_max_col);
        printf("Количество одинаковых элементов в этом столбце: %d\n", max_duplicates);
        
        // Дополнительно: покажем элементы этого столбца
        printf("Элементы столбца %d: ", first_max_col);
        for (int i = 0; i < M; i++) {
            printf("%d ", matrix[i][first_max_col]);
        }
        printf("\n");
    } else {
        printf("Матрица пуста или произошла ошибка\n");
    }
    
    // Освобождение памяти
    for (int i = 0; i < M; i++) {
        free(matrix[i]);
    }
    free(matrix);
    
    return 0;
}
