# 21. WAP to Calculate Reverse of a String Using Recursion

### 1. Problem Understanding

Reverse a string **using recursion** (no loops, no library `strrev()`). The function must call itself, processing the string one character at a time until it reaches the end, and then **build the reversed result on the way back**.

- Input: `"hello"` → Output: `"olleh"`
- Input: `"madam"` → Output: `"madam"` (palindrome — unchanged)

**Key idea:** The *first* character of the string is the *last* character of the reversed string. If the string is `S + ch`, the reversal is `reverse(S) + ch`. Recursion works on the shrinking prefix until the string is empty.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Recursion | Function calls itself with a shorter string (`str + 1`) |
| Base case | Empty string (`'\0'`) — stop recursing, print nothing |
| Post-print order | Characters are printed **after** the recursive call returns — this naturally reverses them |
| Pointer arithmetic | `str + 1` moves the base pointer to the next character |
| String termination | `'\0'` marks the end — the base case |

### 3. Algorithm (Step-by-Step)

**Recursive function `reversePrint(char *str)`:**
1. If `str[0] == '\0'` (empty string) → return (base case).
2. Else:
   - Call `reversePrint(str + 1)` — recurse on the *rest* of the string.
   - **After** the call returns, print `str[0]` (the current first character).

**main():**
1. Read the string.
2. Print `"Original: "` + string.
3. Print `"Reversed: "` and call `reversePrint(str)`.
4. Print newline.

> **Why printing after the call reverses it:** The recursion goes all the way to the end first; as it unwinds, characters come out from last to first.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#include <string.h>

/* recursive function to print a string in reverse */
void reverseString(char *str)
{
    if (str[0] == '\0')          /* base case: empty string */
        return;

    reverseString(str + 1);      /* recurse on the remaining part */

    printf("%c", str[0]);        /* print current character on the way back */
}

int main()
{
    char str[100];

    printf("Enter a string: ");
    scanf("%s", str);            /* or gets(str) for phrases */

    printf("\nOriginal string: %s\n", str);
    printf("Reversed string: ");

    reverseString(str);          /* recursive call */

    printf("\n");

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `void reverseString(char *str)` | Takes a pointer to the current start of the string |
| `if (str[0] == '\0') return;` | **Base case** — empty string, nothing to print; stops recursion |
| `reverseString(str + 1);` | **Recursive call** — moves the pointer one character right each time |
| `printf("%c", str[0]);` | Prints the current first character — executed only **after** the deeper call returns |
| `scanf("%s", str)` | Reads a single word; `str` is already an address so no `&` |

### 6. Dry Run — "hello"

Recursion call stack (going down):

```
reverseString("hello")
  → reverseString("ello")
    → reverseString("llo")
      → reverseString("lo")
        → reverseString("o")
          → reverseString("")        ← base case, return
        print 'o'                    (unwinding)
      print 'l'
    print 'l'
  print 'e'
print 'h'
```

**Printed order during unwinding: o l l e h** → reversed string = **"olleh"** ✓

### 7. Sample Input / Output

**Test 1:**
```
Enter a string: hello

Original string: hello
Reversed string: olleh
```

**Test 2:**
```
Enter a string: madam

Original string: madam
Reversed string: madam
```

**Test 3:**
```
Enter a string: programming

Original string: programming
Reversed string: gnimmargorp
```

### 8. Edge Cases Table

| Input | Reversed | Remark |
|---|---|---|
| "hello" | "olleh" | Normal case |
| "madam" | "madam" | Palindrome — same both ways |
| "a" | "a" | Single character |
| "" (empty) | (nothing) | Base case immediately |
| "12345" | "54321" | Digits reversed like characters |
| "Hello World" (with space) | needs `gets`/`fgets` | `scanf("%s")` reads only "Hello" |

### 9. Common Mistakes to Avoid

1. **Printing the character *before* the recursive call** — that prints the string in its normal order, not reversed. The `printf` must come **after** `reverseString(str + 1)`.
2. **Missing base case** — infinite recursion → stack overflow.
3. **Passing `str++` instead of `str + 1`** — `str++` works here too, but `str + 1` is clearer (no side effect).
4. **Using `%s` in the final printf with a `char *` that isn't null-terminated** — always keep the original string intact.
5. **Forgetting that `scanf("%s", str)` needs no `&`** — adding `&str` passes the wrong type (`char (*)[100]`).
6. **Recursion depth for very long strings** — each call uses stack; a 100,000-char string could overflow. Fine for normal inputs.

### 10. Alternative — recursion that *returns* the reversed string

```c
#include <stdio.h>
#include <string.h>

/* recursive function that stores reversed result into out[] */
void reverseInto(char *str, char *out, int pos)
{
    int len = strlen(str);

    if (pos < 0)                 /* all characters processed */
    {
        out[len] = '\0';         /* terminate the result */
        return;
    }

    out[len - 1 - pos] = str[pos];  /* last char goes first */

    reverseInto(str, out, pos - 1);
}

int main()
{
    char str[100], rev[100];

    printf("Enter a string: ");
    scanf("%s", str);

    reverseInto(str, rev, strlen(str) - 1);

    printf("\nOriginal string: %s\n", str);
    printf("Reversed string: %s\n", rev);

    return 0;
}
```

**Recursion using string length (also common in lab records):**
```c
#include <stdio.h>
#include <string.h>

void reverseString(char *str)
{
    int len = strlen(str);

    if (len == 0)                /* base case */
        return;

    printf("%c", str[len - 1]);  /* print last character */
    str[len - 1] = '\0';         /* temporarily remove it */
    reverseString(str);          /* recurse on the shorter string */
}

int main()
{
    char str[100];
    printf("Enter a string: ");
    scanf("%s", str);
    reverseString(str);
    printf("\n");
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Original | Reversed |
|---|---|
| hello | olleh |
| madam | madam |
| programming | gnimmargorp |
| a | a |
| 12345 | 54321 |

---
