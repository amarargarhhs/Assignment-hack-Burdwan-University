# 5. Write a Function That Checks Whether a Given String Is a Palindrome or Not

### 1. Problem Understanding

A **palindrome** is a word, phrase, or sequence that reads the **same forward and backward**:

- `"madam"` → reverse is `"madam"` ✓ palindrome
- `"racecar"` → palindrome ✓
- `"hello"` → reverse is `"olleh"` ✗ not a palindrome

We must write a **function** (called from `main()`) that:
- **Accepts** a string.
- **Checks** whether it reads the same from both ends.
- **Returns** a result (commonly `1` for palindrome, `0` for not) that `main()` then prints.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| String (`char[]`) | A C string is a character array terminated by `'\0'` |
| `strlen()` | Returns the length of the string (from `<string.h>`) |
| Two-pointer technique | Compare characters from the **front** and the **back**, moving inward |
| Function with return value | The function returns an integer status (1 = palindrome, 0 = not) |

### 3. Algorithm (Step-by-Step)

1. Start.
2. `main()` reads the string into a character array.
3. Call the function `isPalindrome(string)`.
4. **Inside the function:**
   - `length = strlen(str)`.
   - Set `i = 0`, `j = length - 1`.
   - **While `i < j`:**
     - If `str[i] != str[j]` → return **0** (not a palindrome).
     - Otherwise move inward: `i++`, `j--`.
   - If the loop completes, return **1** (palindrome).
5. `main()` prints the verdict based on the returned value.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#include <string.h>

/* function that checks for palindrome:
   returns 1 if palindrome, 0 otherwise */
int isPalindrome(char str[])
{
    int i = 0;
    int j = strlen(str) - 1;

    while (i < j)
    {
        if (str[i] != str[j])
            return 0;          /* mismatch found -> not a palindrome */
        i++;
        j--;
    }

    return 1;                  /* all pairs matched -> palindrome */
}

int main()
{
    char str[100];

    printf("Enter a string: ");
    scanf("%s", str);          /* or gets(str) — see note below */

    if (isPalindrome(str) == 1)
        printf("\n\"%s\" is a PALINDROME.\n", str);
    else
        printf("\n\"%s\" is NOT a palindrome.\n", str);

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int isPalindrome(char str[])` | Function signature — accepts a string, returns `int` (1/0) |
| `int j = strlen(str) - 1;` | `j` points to the **last** character (index length−1) |
| `while (i < j)` | Compare only until the pointers meet/cross — each pair compared once |
| `if (str[i] != str[j]) return 0;` | Any mismatch immediately proves it is **not** a palindrome |
| `i++; j--;` | Move one step inward from each end |
| `return 1;` | Reached only if every pair matched → palindrome |
| `scanf("%s", str)` | Reads a single word (no spaces) into the array. `str` is already the address, so **no `&`** is needed |

> **Note on input:** `scanf("%s", str)` reads only up to the first space. If the assignment expects phrases (e.g., "madam" only), this is fine. To read a phrase with spaces, use `gets(str)` (old) or `fgets(str, 100, stdin)` (recommended, but it keeps the trailing newline — strip it first).

### 6. Dry Run

**Input: "madam"** (length 5, so i=0, j=4)

| i | j | str[i] | str[j] | Match? |
|---|---|---|---|---|
| 0 | 4 | m | m | ✓ |
| 1 | 3 | a | a | ✓ |
| 2 | 2 | d | d | ✓ (i==j, loop condition fails next) |

Loop ends → return 1 → **PALINDROME** ✓

**Input: "hello"** (length 5, i=0, j=4)

| i | j | str[i] | str[j] | Match? |
|---|---|---|---|---|
| 0 | 4 | h | o | ✗ → return 0 |

→ **NOT a palindrome** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter a string: madam

"madam" is a PALINDROME.
```

**Test 2:**
```
Enter a string: hello

"hello" is NOT a palindrome.
```

**Test 3:**
```
Enter a string: racecar

"racecar" is a PALINDROME.
```

### 8. Edge Cases Table

| Input | Verdict | Reason |
|---|---|---|
| "madam" | Palindrome | Reads same forward/backward |
| "racecar" | Palindrome | Reads same forward/backward |
| "hello" | Not | Mismatch at first pair |
| Single char "a" | Palindrome | i==j immediately; trivially true |
| Empty string "" | Palindrome | j = −1; `i < j` never true → returns 1 (mathematically correct) |
| "Madam" (capital M) | **Not** | Case-sensitive comparison: `'M' != 'm'` |
| "noon" | Palindrome | Symmetric |

> **Case-sensitivity:** The function above is **case-sensitive** (`"Madam"` fails). If the assignment wants case-insensitive checking, compare using `toupper()`/`tolower()` from `<ctype.h>` (see alternative below).

### 9. Common Mistakes to Avoid

1. **Using `&` in `scanf("%s", &str)`** — a string variable is already a pointer; `&` gives the wrong type.
2. **Forgetting `#include <string.h>`** for `strlen()`.
3. **Writing the function but never calling it** — the assignment says "write a function", so `main()` must call it.
4. **Using `i <= j`** in the loop — would compare the middle character with itself (harmless but unnecessary); `i < j` is cleaner.
5. **Modifying the string inside the function** if it was meant to stay unchanged.
6. **Assuming `==` compares the whole string** — in C, `str[i] == str[j]` compares single *characters*, which is exactly what we want here.

### 10. Alternative Versions

**Version with `strrev()` (using `<string.h>`):**
```c
#include <stdio.h>
#include <string.h>

int isPalindrome(char str[])
{
    char temp[100];
    strcpy(temp, str);          /* copy the original */
    if (strcmp(str, strrev(temp)) == 0)
        return 1;
    return 0;
}
```
*(Note: `strrev()` is not standard C — works in many compilers like Turbo C/Borland, but avoid it on modern GCC.)*

**Case-insensitive version (using `<ctype.h>`):**
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int isPalindrome(char str[])
{
    int i = 0, j = strlen(str) - 1;
    while (i < j)
    {
        if (tolower(str[i]) != tolower(str[j]))
            return 0;
        i++;
        j--;
    }
    return 1;
}
```

**Recursive version (advanced):**
```c
#include <stdio.h>
#include <string.h>

int isPalindrome(char str[], int i, int j)
{
    if (i >= j)
        return 1;                      /* all checked -> palindrome */
    if (str[i] != str[j])
        return 0;                      /* mismatch -> not palindrome */
    return isPalindrome(str, i + 1, j - 1);   /* check inner part */
}
```

### 11. Brief Result Table (for the lab record)

| Input string | Palindrome? |
|---|---|
| madam | Yes |
| racecar | Yes |
| noon | Yes |
| hello | No |
| programming | No |
| a | Yes |

---
