# 17. Given Two Ordered Arrays of Integers, Write a Program to Merge Them Into One Ordered Array

### 1. Problem Understanding

Merge two arrays that are **already sorted in ascending order** into a **single sorted array**, without re-sorting the whole result. This is the classic **merge** operation from *merge sort*.

Example:
```
A = {1, 3, 5, 7}
B = {2, 4, 6, 8, 10}
C = {1, 2, 3, 4, 5, 6, 7, 8, 10}
```

**Why a simple copy-then-sort is not the point:** The assignment wants the **merge technique** — a single linear pass (O(n+m)) using a three-pointer approach, not sorting the combined array (O(n log n)).

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| Sorted arrays | Both inputs already ascending |
| Three indices | `i` for A, `j` for B, `k` for result C |
| Two-way merge | Compare `A[i]` vs `B[j]`, copy the smaller one, advance that side |
| Leftover handling | When one array is exhausted, copy the rest of the other |
| Result size | `n + m` |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read size `n` and elements of array A (ascending).
3. Read size `m` and elements of array B (ascending).
4. Set `i = 0, j = 0, k = 0`.
5. **While `i < n && j < m`:**
   - If `A[i] <= B[j]` → `C[k++] = A[i++]`.
   - Else → `C[k++] = B[j++]`.
6. **While `i < n`:** copy remaining A elements → `C[k++] = A[i++]`.
7. **While `j < m`:** copy remaining B elements → `C[k++] = B[j++]`.
8. Print the merged array C (size `n + m`).
9. Stop.

### 4. The C Program (Source Code)

```c
#include <stdio.h>

void mergeArrays(int a[], int n, int b[], int m, int c[])
{
    int i = 0, j = 0, k = 0;

    /* compare and copy the smaller element */
    while (i < n && j < m)
    {
        if (a[i] <= b[j])
            c[k++] = a[i++];
        else
            c[k++] = b[j++];
    }

    /* copy remaining elements of a (if any) */
    while (i < n)
        c[k++] = a[i++];

    /* copy remaining elements of b (if any) */
    while (j < m)
        c[k++] = b[j++];
}

int main()
{
    int a[50], b[50], c[100];
    int n, m, i;

    /* read first array */
    printf("Enter number of elements in first array: ");
    scanf("%d", &n);
    printf("Enter %d sorted elements: ", n);
    for (i = 0; i < n; i++)
        scanf("%d", &a[i]);

    /* read second array */
    printf("Enter number of elements in second array: ");
    scanf("%d", &m);
    printf("Enter %d sorted elements: ", m);
    for (i = 0; i < m; i++)
        scanf("%d", &b[i]);

    /* merge */
    mergeArrays(a, n, b, m, c);

    /* display result */
    printf("\nMerged sorted array:\n");
    for (i = 0; i < n + m; i++)
        printf("%d ", c[i]);
    printf("\n");

    return 0;
}
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `int i = 0, j = 0, k = 0;` | Indices into A, B, and result C |
| `while (i < n && j < m)` | Main loop — runs while **both** arrays still have elements |
| `if (a[i] <= b[j]) c[k++] = a[i++];` | Smaller element goes into C; advance both source and destination |
| `else c[k++] = b[j++];` | Otherwise take from B |
| `while (i < n) c[k++] = a[i++];` | After B is exhausted, copy A's leftovers |
| `while (j < m) c[k++] = b[j++];` | After A is exhausted, copy B's leftovers |
| `for (i = 0; i < n + m; i++)` | Print all `n + m` merged elements |

### 6. Dry Run

**A = {1, 3, 5, 7}, B = {2, 4, 6, 8, 10}**

| Step | i | j | Compare | Copy to C | k |
|---|---|---|---|---|---|
| 1 | 0 | 0 | 1 ≤ 2 → A | 1 | 1 |
| 2 | 1 | 0 | 3 ≤ 2? no → B | 2 | 2 |
| 3 | 1 | 1 | 3 ≤ 4 → A | 3 | 3 |
| 4 | 2 | 1 | 5 ≤ 4? no → B | 4 | 4 |
| 5 | 2 | 2 | 5 ≤ 6 → A | 5 | 5 |
| 6 | 3 | 2 | 7 ≤ 6? no → B | 6 | 6 |
| 7 | 3 | 3 | 7 ≤ 8 → A | 7 | 7 |
| 8 | 4 | 3 | A done → copy B | 8 | 8 |
| 9 | 4 | 4 | — → copy B | 10 | 9 |

**C = {1, 2, 3, 4, 5, 6, 7, 8, 10}** ✓ (ascending, all 9 elements)

### 7. Sample Input / Output

**Test 1:**
```
Enter number of elements in first array: 4
Enter 4 sorted elements: 1 3 5 7
Enter number of elements in second array: 5
Enter 5 sorted elements: 2 4 6 8 10

Merged sorted array:
1 2 3 4 5 6 7 8 10
```

**Test 2 (duplicates):**
```
Enter number of elements in first array: 3
Enter 3 sorted elements: 2 4 6
Enter number of elements in second array: 3
Enter 3 sorted elements: 1 2 3

Merged sorted array:
1 2 2 3 4 6
```
*(Note: duplicate 2 appears twice — merge preserves all elements.)*

### 8. Edge Cases Table

| Case | Behaviour |
|---|---|
| One array empty (n=0 or m=0) | The first `while` is skipped; leftovers copy the other array whole |
| Equal elements | `<=` copies A's first — both still end up in C (stable merge) |
| All A elements smaller than B | Merge = A followed by B |
| All B elements smaller than A | Merge = B followed by A |
| Negative numbers | Merged correctly in ascending order |
| Same size arrays | Merges symmetrically |

### 9. Common Mistakes to Avoid

1. **Forgetting the two leftover loops** — if A or B is exhausted first, the remaining elements would be lost, producing a wrong (shorter) array.
2. **Using `a[i] < b[j]` instead of `<=`** — equal values from A would be skipped entirely (both `a[i]==b[j]` and `b[j]==a[i]` would fail the strict test). Use `<=` or handle equality.
3. **Reversing the comparison** — copying the larger element first destroys the ascending order.
4. **Size of result array too small** — must be at least `n + m` (here 100).
5. **Off-by-one in the print loop** — print `n + m` elements (indices 0 to n+m−1).
6. **Assuming input is sorted without checking** — the algorithm is only correct for ordered inputs.

### 10. Alternative Version — input in any order but merge still valid

**Non-decreasing order with `>=` for the else-branch clarity:**
```c
while (i < n && j < m)
{
    if (a[i] <= b[j])
        c[k] = a[i++];
    else
        c[k] = b[j++];
    k++;
}
```

**Merge as a complete program without a function (lab-record style):**
```c
#include <stdio.h>

int main()
{
    int a[50], b[50], c[100];
    int n, m, i = 0, j = 0, k = 0;

    printf("Enter n and m: ");
    scanf("%d %d", &n, &m);

    printf("Enter %d sorted elements of A: ", n);
    for (k = 0; k < n; k++) scanf("%d", &a[k]);
    printf("Enter %d sorted elements of B: ", m);
    for (k = 0; k < m; k++) scanf("%d", &b[k]);

    k = 0;
    while (i < n && j < m)
    {
        if (a[i] <= b[j]) c[k++] = a[i++];
        else              c[k++] = b[j++];
    }
    while (i < n) c[k++] = a[i++];
    while (j < m) c[k++] = b[j++];

    printf("\nMerged sorted array: ");
    for (i = 0; i < n + m; i++)
        printf("%d ", c[i]);
    printf("\n");
    return 0;
}
```

### 11. Brief Result Table (for the lab record)

| Array A | Array B | Merged C |
|---|---|---|
| 1 3 5 7 | 2 4 6 8 10 | 1 2 3 4 5 6 7 8 10 |
| 2 4 6 | 1 2 3 | 1 2 2 3 4 6 |
| 10 20 30 | 5 15 | 5 10 15 20 30 |

---
