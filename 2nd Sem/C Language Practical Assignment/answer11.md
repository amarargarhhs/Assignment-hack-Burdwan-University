# 11. WAP That Prints a Table Indicating the Number of Occurrences of Each Alphabet in Text Entered as Command Line Arguments

### 1. Problem Understanding

This program receives **text via command-line arguments** (`argc` and `argv[]` of `main`) and prints a **frequency table** showing how many times **each alphabet letter (A–Z)** appears.

Example invocation:
```
myprog.exe Hello World Welcome
```
Counts each letter across all three words (ignoring spaces and punctuation):
- H→1, E→2, L→3, O→2, W→2, R→1, D→1, M→1, C→1, etc.

**Key details:**
- The program name itself is `argv[0]` and is **not** counted.
- Arguments start at `argv[1]` and go up to `argv[argc-1]`.
- Counts should be **case-insensitive** (`H` and `h` both count as H) — the table is usually shown in uppercase.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `main(int argc, char *argv[])` | Command-line interface — `argc` = argument count, `argv` = argument array |
| Character counting | An array `count[26]` where `count[0]`→A, `count[1]`→B, … `count[25]`→Z |
| `tolower()` / `toupper()` | Normalise case (from `<ctype.h>`) |
| Mapping char → index | `ch - 'a'` (or `ch - 'A'`) converts a letter into a 0–25 index |
| Table formatting | `printf("%c : %d\n", ...)` for a clean two-column output |

### 3. Algorithm (Step-by-Step)

1. Start. Program run as `prog word1 word2 ...`.
2. Initialise `count[26]` to all zeros.
3. **For each argument `i` from 1 to argc-1:**
   - **For each character `ch` in that argument:**
     - Convert `ch` to lowercase.
     - If `ch` is between `'a'` and `'z'` → `count[ch - 'a']++`.
4. Print heading `Alphabet : Frequency`.
5. **For `i = 0` to 25:** if `count[i] > 0`, print `'A'+i : count[i]`.
6. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>
#include <ctype.h>      /* for tolower() */

int main(int argc, char *argv[])
{
    int count[26] = {0};        /* counts for A..Z (index 0..25) */
    int i, j;

    if (argc < 2)               /* no text supplied */
    {
        printf("Usage: %s <text...>\n", argv[0]);
        printf("Example: %s Hello World\n", argv[0]);
        return 1;
    }

    /* loop through every command-line argument */
    for (i = 1; i < argc; i++)
    {
        /* loop through every character of the current argument */
        for (j = 0; argv[i][j] != '\0'; j++)
        {
            char ch = tolower(argv[i][j]);   /* normalise case */

            if (ch >= 'a' && ch <= 'z')      /* only alphabets */
                count[ch - 'a']++;
        }
    }

    /* print the frequency table */
    printf("\nAlphabet  Frequency\n");
    printf("------------------\n");
    for (i = 0; i < 26; i++)
    {
        if (count[i] > 0)
            printf("  %c        %d\n", 'A' + i, count[i]);
    }

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int main(int argc, char *argv[])` | `argc` = number of arguments (incl. program name), `argv[]` = array of strings |
| `int count[26] = {0};` | Frequency table — index 0 = A, index 25 = Z |
| `if (argc < 2)` | Nothing after the program name → print usage and exit |
| `for (i = 1; i < argc; i++)` | Skip `argv[0]` (program name); count only user text |
| `for (j = 0; argv[i][j] != '\0'; j++)` | Walk through each character of the current argument string |
| `tolower(argv[i][j])` | Makes counting case-insensitive (`H` and `h` both → `h`) |
| `if (ch >= 'a' && ch <= 'z')` | Reject digits, punctuation, spaces — only count letters |
| `count[ch - 'a']++` | Maps `'a'`→0, `'b'`→1, …, `'z'`→25 and increments that counter |
| `'A' + i` | Converts index back to an uppercase letter for display |

### 6. Dry Run

**Command:** `freq.exe Hello World`

| Arg | Char | tolower | index | count updated |
|---|---|---|---|---|
| Hello | H | h | 7 | H=1 |
| | e | e | 4 | E=1 |
| | l | l | 11 | L=1 |
| | l | l | 11 | L=2 |
| | o | o | 14 | O=1 |
| World | W | w | 22 | W=1 |
| | o | o | 14 | O=2 |
| | r | r | 17 | R=1 |
| | l | l | 11 | L=3 |
| | d | d | 3 | D=1 |

**Output table:**
```
Alphabet  Frequency
------------------
  D        1
  E        1
  H        1
  L        3
  O        2
  R        1
  W        1
```

### 7. Sample Input / Output

**Test 1:**
```
C:\> freq.exe Madam racecar
Alphabet  Frequency
------------------
  A        4
  C        2
  D        1
  E        1
  M        1
  R        2
```

**Test 2 (no text):**
```
C:\> freq.exe
Usage: freq.exe <text...>
Example: freq.exe Hello World
```

**Test 3 (punctuation ignored):**
```
C:\> freq.exe "Hello, World!"
Alphabet  Frequency
------------------
  D        1
  E        1
  H        1
  L        3
  O        2
  R        1
  W        1
```
*(Comma, space, exclamation are ignored; quoted string keeps spaces inside one argument.)*

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| Uppercase + lowercase mix (`Hello Hello`) | Counted together as one letter (case-insensitive) |
| Digits / punctuation (`123 !?`) | Ignored — not alphabets |
| No arguments | Usage message printed, program exits with code 1 |
| Argument with spaces (`"hello world"`) | Treated as one argument when quoted |
| Empty string argument | Inner loop never runs — nothing counted |
| Letters with accents (`é`, `ñ`) | `tolower` leaves them outside `'a'..'z'` → ignored |

### 9. Common Mistakes to Avoid

1. **Starting the argument loop at 0** — that counts the **program name** (`argv[0]`) too. Start at **1**.
2. **Forgetting to reset counts** — if the table isn't zero-initialised (`= {0}`), counts contain garbage.
3. **Case-sensitive counting** — `H` and `h` end up in different buckets unless you normalise with `tolower()`/`toupper()`.
4. **`ch - 'a'` on a non-letter** — gives an index outside 0–25 and corrupts memory; always guard with the `'a' <= ch <= 'z'` check first.
5. **Using `%s` to print** — the program takes input from the **command line**, not from `scanf`.
6. **Off-by-one on `argc`** — valid argument strings are `argv[1]` … `argv[argc-1]`; that's exactly `argc-1` strings.

### 10. Alternative Version — using `strlen` instead of `'\0'` check

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main(int argc, char *argv[])
{
    int count[26] = {0};
    int i, j, len;

    if (argc < 2)
    {
        printf("Usage: %s <text...>\n", argv[0]);
        return 1;
    }

    for (i = 1; i < argc; i++)
    {
        len = strlen(argv[i]);
        for (j = 0; j < len; j++)
        {
            char ch = tolower(argv[i][j]);
            if (ch >= 'a' && ch <= 'z')
                count[ch - 'a']++;
        }
    }

    printf("\nAlphabet  Frequency\n------------------\n");
    for (i = 0; i < 26; i++)
        if (count[i] > 0)
            printf("  %c        %d\n", 'A' + i, count[i]);

    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Command-line input | Letter with max frequency | Letters found |
|---|---|---|
| `freq.exe Hello World` | L (3) | D, E, H, L, O, R, W |
| `freq.exe Madam racecar` | A (4) | A, C, D, E, M, R |
| `freq.exe` (no args) | — | Usage message shown |

---
