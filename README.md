## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER


## NAME : YUGABHARATHI T
## REGISTER.NO : 212224040375
 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char matrix[5][5];

void createMatrix(char key[]) {
    int used[26] = {0};
    int i, k = 0;
    char ch;

    for (i = 0; key[i] != '\0'; i++) {
        ch = toupper(key[i]);
        if (ch == 'J') ch = 'I';

        if (ch >= 'A' && ch <= 'Z' && !used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }

    for (ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue;
        if (!used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            k++;
        }
    }
}

void printMatrix() {
    int i, j;
    printf("\nKey Matrix:\n");
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }
}

void findPos(char ch, int *r, int *c) {
    int i, j;
    if (ch == 'J') ch = 'I';

    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (matrix[i][j] == ch) {
                *r = i;
                *c = j;
                return;
            }
}

void encrypt(char text[]) {
    int i, r1, c1, r2, c2;

    printf("\nCiphertext/Encrypted text: ");
    for (i = 0; text[i] && text[i + 1]; i += 2) {
        findPos(text[i], &r1, &c1);
        findPos(text[i + 1], &r2, &c2);

        if (r1 == r2)
            printf("%c%c ",
                   matrix[r1][(c1 + 1) % 5],
                   matrix[r2][(c2 + 1) % 5]);
        else if (c1 == c2)
            printf("%c%c ",
                   matrix[(r1 + 1) % 5][c1],
                   matrix[(r2 + 1) % 5][c2]);
        else
            printf("%c%c ",
                   matrix[r1][c2],
                   matrix[r2][c1]);
    }
}

int main() {
    char text[100], key[50];
    int i, j = 0;

    printf("Enter plaintext: ");
    fgets(text, sizeof(text), stdin);

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);

    text[strcspn(text, "\n")] = '\0';
    key[strcspn(key, "\n")] = '\0';

    for (i = 0; text[i] != '\0'; i++)
        if (isalpha(text[i]))
            text[j++] = toupper(text[i]);
    text[j] = '\0';

    if (strlen(text) % 2 != 0)
        strcat(text, "X");

    createMatrix(key);
    printMatrix();
    encrypt(text);

}

```





## Output:

<img width="1696" height="867" alt="Screenshot 2026-01-29 205602" src="https://github.com/user-attachments/assets/cc482b01-cf28-452e-9f19-48d45349cfea" />

