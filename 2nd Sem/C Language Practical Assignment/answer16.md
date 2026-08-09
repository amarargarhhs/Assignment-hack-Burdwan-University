# 16. Menu-Driven Program to Perform Operations on Strings

### 1. Problem Understanding

Write a **menu-driven** program that lets the user repeatedly choose one of nine string operations:

| # | Operation |
|---|---|
| 1 | Show **address** of each character |
| 2 | **Concatenate** without `strcat` |
| 3 | **Concatenate** with `strcat` |
| 4 | **Compare** two strings |
| 5 | Calculate **length** (using pointers) |
| 6 | Convert **lowercase → uppercase** |
| 7 | Convert **uppercase → lowercase** |
| 8 | **Count vowels** |
| 9 | **Reverse** the string |

The program should read one (or two, where needed) strings, show a menu, and dispatch each choice to its own function using a `switch`.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Strings (`char[]`) | Null-terminated character arrays |
| `switch` menu | Dispatching user choice to a function |
| Pointers | Address printing, and length via pointer traversal |
| Manual string logic | Hand-rolled `strlen`, `strcat`, `strcmp`, `strrev` without library functions |
| ASCII math | `'a' - 32 == 'A'` (or `tolower`/`toupper`) |
| Vowel set | a, e, i, o, u (both cases) |

### 3. Program Design

```
main()
 ├── declare str[100], str2[100]
 ├── enter main string (and second string)
 ├── do { show menu → switch(choice)
 │       1: address of each character
 │       2: concat without strcat (str + str2 → result)
 │       3: concat with strcat
 │       4: compare two strings
 │       5: length using pointers
 │       6: lowercase → uppercase
 │       7: uppercase → lowercase
 │       8: count vowels
 │       9: reverse string
 │       0: quit
 └── } while (choice != 0)
```

### 4. The C Program (Source Code)

```c
#include <stdio.h>

/* 1. show address of each character */
void showAddresses(char s[])
{
    int i = 0;
    printf("\nCharacter  Address\n");
    while (s[i] != '\0')
    {
        printf("  %c         %p\n", s[i], (void *)&s[i]);
        i++;
    }
}

/* 2. concatenate WITHOUT strcat (manual) */
void concatManual(char s1[], char s2[], char result[])
{
    int i = 0, j = 0;

    while (s1[i] != '\0')        /* copy first string */
    {
        result[i] = s1[i];
        i++;
    }
    while (s2[j] != '\0')        /* append second string */
    {
        result[i] = s2[j];
        i++;
        j++;
    }
    result[i] = '\0';            /* terminate result */

    printf("\nConcatenated (manual) = %s\n", result);
}

/* 3. concatenate WITH strcat */
void concatLib(char s1[], char s2[], char result[])
{
    int i = 0;

    while (s1[i] != '\0')        /* copy first into result */
    {
        result[i] = s1[i];
        i++;
    }
    result[i] = '\0';

    strcat(result, s2);          /* library function appends s2 */

    printf("\nConcatenated (strcat) = %s\n", result);
}

/* 4. compare two strings (manual strcmp) */
void compareStrings(char s1[], char s2[])
{
    int i = 0, result = 0;

    while (s1[i] != '\0' && s2[i] != '\0')
    {
        if (s1[i] != s2[i])
        {
            result = s1[i] - s2[i];
            break;
        }
        i++;
    }

    if (result == 0 && s1[i] != s2[i])      /* one string is a prefix of the other */
        result = s1[i] - s2[i];

    if (result == 0)
        printf("\nStrings are EQUAL.\n");
    else if (result < 0)
        printf("\nFirst string is SMALLER than second.\n");
    else
        printf("\nFirst string is LARGER than second.\n");
}

/* 5. calculate length USING POINTERS */
int stringLength(char *s)
{
    int len = 0;
    while (*s != '\0')
    {
        len++;
        s++;                    /* move pointer forward */
    }
    return len;
}

/* 6. lowercase -> uppercase */
void toUpper(char s[])
{
    int i = 0;
    while (s[i] != '\0')
    {
        if (s[i] >= 'a' && s[i] <= 'z')
            s[i] = s[i] - 32;   /* ASCII difference between a and A */
        i++;
    }
    printf("\nUppercase string = %s\n", s);
}

/* 7. uppercase -> lowercase */
void toLower(char s[])
{
    int i = 0;
    while (s[i] != '\0')
    {
        if (s[i] >= 'A' && s[i] <= 'Z')
            s[i] = s[i] + 32;
        i++;
    }
    printf("\nLowercase string = %s\n", s);
}

/* 8. count vowels */
void countVowels(char s[])
{
    int i = 0, count = 0;
    char ch;

    while (s[i] != '\0')
    {
        ch = s[i];
        if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' ||
            ch == 'A' || ch == 'E' || ch == 'I' || ch == 'O' || ch == 'U')
            count++;
        i++;
    }
    printf("\nNumber of vowels = %d\n", count);
}

/* 9. reverse the string (manual, in-place) */
void reverseString(char s[])
{
    int i = 0, j = 0;
    char temp;

    while (s[j] != '\0')          /* find length */
        j++;
    j--;                          /* point to last character */

    while (i < j)                 /* swap from both ends */
    {
        temp = s[i];
        s[i] = s[j];
        s[j] = temp;
        i++;
        j--;
    }
    printf("\nReversed string = %s\n", s);
}

int main()
{
    char str[100], str2[100], result[200];
    int choice, n;

    printf("Enter a string: ");
    scanf("%s", str);
    printf("Enter a second string: ");
    scanf("%s", str2);

    do {
        printf("\n------------ STRING MENU ------------\n");
        printf("1. Show address of each character\n");
        printf("2. Concatenate (without strcat)\n");
        printf("3. Concatenate (with strcat)\n");
        printf("4. Compare two strings\n");
        printf("5. Calculate length (using pointers)\n");
        printf("6. Convert lowercase to uppercase\n");
        printf("7. Convert uppercase to lowercase\n");
        printf("8. Count vowels\n");
        printf("9. Reverse the string\n");
        printf("0. Quit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:  showAddresses(str);                    break;
            case 2:  concatManual(str, str2, result);       break;
            case 3:  concatLib(str, str2, result);          break;
            case 4:  compareStrings(str, str2);             break;
            case 5:  n = stringLength(str);
                     printf("\nLength of \"%s\" = %d\n", str, n);  break;
            case 6:  toUpper(str);                          break;
            case 7:  toLower(str);                          break;
            case 8:  countVowels(str);                      break;
            case 9:  reverseString(str);                    break;
            case 0:  printf("Exiting...\n");                break;
            default: printf("Invalid choice! Try again.\n");
        }
    } while (choice != 0);

    return 0;
}
```

> **Note:** This uses `strcat` in case 3, so include `<string.h>` at the top: `#include <string.h>`. (Some compilers auto-include, but it is best practice.)

### 5. Line-by-Line Explanation of Each Function

| Function | Key logic |
|---|---|
| `showAddresses` | Prints `&s[i]` with `%p` format specifier (address of each character) |
| `concatManual` | Two loops: copy `s1` into result, then append `s2`; terminate with `'\0'` |
| `concatLib` | Copies `s1`, then calls library `strcat(result, s2)` |
| `compareStrings` | Compares char by char; on mismatch `result = s1[i] - s2[i]` (like `strcmp`); handles prefix case |
| `stringLength` | Walks the pointer `*s` until `'\0'`, counting each step — **pointer traversal** |
| `toUpper` | `s[i] - 32` converts `'a'`(97) → `'A'`(65); only for lowercase letters |
| `toLower` | `s[i] + 32` converts `'A'`(65) → `'a'`(97) |
| `countVowels` | Compares each char against a/e/i/o/u in both cases |
| `reverseString` | Finds the end, then swaps characters from both ends moving inward |

### 6. Dry Run

**String: "Hello" (len 5)**

**Reverse:**
| i | j | s[i] | s[j] | After swap |
|---|---|---|---|---|
| 0 | 4 | H | o | o l l e H |
| 1 | 3 | e | l | o l l e H |
| 2 | 2 | l | l | stop (i>=j) |

Result: **"olleH"** ✓

**Uppercase (ASCII):** H(72) e(101) l(108) l(108) o(111)
- e → 101−32 = 69 = 'E', l → 76 = 'L', o → 79 = 'O' → **"HELLO"** ✓

**Length via pointer:** pointer steps H→e→l→l→o→'\0' → 5 steps → **length 5** ✓

**Compare "abc" vs "abd":**
- i=0: a==a, i=1: b==b, i=2: c != d → result = 'c'−'d' = −1 → "smaller" ✓

### 7. Sample Run

```
Enter a string: Hello
Enter a second string: World

------------ STRING MENU ------------
...
Enter your choice: 1
Character  Address
  H         0x7ffc1a2b3040
  e         0x7ffc1a2b3041
  l         0x7ffc1a2b3042
  l         0x7ffc1a2b3043
  o         0x7ffc1a2b3044
...
Enter your choice: 2
Concatenated (manual) = HelloWorld

Enter your choice: 8
Number of vowels = 2
```

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| Empty string | Length 0; reverse = empty; vowels = 0 |
| All uppercase input + `toUpper` | No change (already uppercase) |
| Digits/symbols (`"123!@#"`) | Unaffected by case conversions; not vowels |
| Equal strings | Compare → "EQUAL" |
| Prefix strings ("abc" vs "abcde") | Handled by the extra check → "SMALLER" |
| Mixed case ("HeLLo") | Vowel count still correct (both cases checked) |

### 9. Common Mistakes to Avoid

1. **Forgetting `'\0'` termination** in manual concatenation → garbage output.
2. **`scanf("%s")` with spaces** — only reads one word; use `gets`/`fgets` for phrases.
3. **Integer overflow in compare** — keep `result` as `int`, fine for ASCII differences.
4. **Off-by-one in reverse** — `j--` after finding `'\0'` (the last char is at index len−1).
5. **Using `%s` with a single char** — addresses must be printed with `%p`, chars with `%c`.
6. **Calling `strcat` without `<string.h>`** — implicit declaration warning.
7. **Forgetting that `toUpper`/`toLower` modify the original string** — the change persists in `main` (arrays pass by reference).

### 10. Brief Result Table (for the lab record)

| Operation | Input "Hello", "World" | Result |
|---|---|---|
| Address of each char | H,e,l,l,o | one address per character |
| Concatenate (manual) | Hello + World | HelloWorld |
| Concatenate (strcat) | Hello + World | HelloWorld |
| Compare | Hello vs World | H < W → "SMALLER" |
| Length (pointers) | Hello | 5 |
| Lower→Upper | Hello | HELLO |
| Upper→Lower | Hello | hello |
| Count vowels | Hello | 2 |
| Reverse | Hello | olleH |

---
