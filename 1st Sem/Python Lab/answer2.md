# 2. WAP to Calculate Total Marks, Percentage and Grade of a Student

### 1. Problem Understanding

The program must:
1. Read **marks obtained in three subjects** from the user.
2. Calculate **total marks** = sum of the three subject marks.
3. Calculate **percentage** = (total / maximum possible marks) × 100.
   - If each subject is out of 100, the maximum is 300.
4. Assign a **grade** using this criteria:

| Grade | Percentage (P) |
|---|---|
| A | P ≥ 80 |
| B | 70 ≤ P < 80 |
| C | 60 ≤ P < 70 |
| D | 40 ≤ P < 60 |
| E | P < 40 |

**Assumption:** each subject is out of **100 marks**, so maximum total = 300.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `input()` + `float()` | Reads the three subject marks |
| Accumulator / direct sum | `total = m1 + m2 + m3` |
| Percentage formula | `(total / 300) * 100` |
| `if/elif/else` chain | Grade decision — order matters (check A first, then B, etc.) |
| `:.2f` formatting | Prints percentage with 2 decimals |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read marks of **subject 1**, **subject 2**, **subject 3**.
3. `total = m1 + m2 + m3`.
4. `percentage = (total / 300) * 100`.
5. **Decide grade:**
   - If `percentage >= 80` → grade A
   - Else if `percentage >= 70` → grade B
   - Else if `percentage >= 60` → grade C
   - Else if `percentage >= 40` → grade D
   - Else → grade E
6. Print total, percentage, and grade.
7. Stop.

### 4. The Python Program (Source Code)

```python
# Calculate total marks, percentage and grade of a student

def calculate_grade(percentage):
    """Return the grade based on the percentage."""
    if percentage >= 80:
        return 'A'
    elif percentage >= 70:
        return 'B'
    elif percentage >= 60:
        return 'C'
    elif percentage >= 40:
        return 'D'
    else:
        return 'E'

def main():
    print("Enter marks obtained in three subjects (each out of 100):")
    m1 = float(input("Subject 1: "))
    m2 = float(input("Subject 2: "))
    m3 = float(input("Subject 3: "))

    total = m1 + m2 + m3
    percentage = (total / 300) * 100

    grade = calculate_grade(percentage)

    print("\n----------- RESULT -----------")
    print(f"Total Marks    : {total:.2f} / 300")
    print(f"Percentage     : {percentage:.2f} %")
    print(f"Grade          : {grade}")
    print("-------------------------------")

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `def calculate_grade(percentage):` | User-defined function that maps percentage → grade |
| `if percentage >= 80: return 'A'` | Highest band checked **first** |
| `elif percentage >= 70: return 'B'` | Reached only if < 80 → safe to check ≥ 70 only |
| `elif ... >= 40: return 'D'` | The chain keeps narrowing the range |
| `else: return 'E'` | Anything below 40 → E |
| `m1 = float(input("Subject 1: "))` | Reads each subject's marks as a float |
| `total = m1 + m2 + m3` | Sum of the three subjects |
| `percentage = (total / 300) * 100` | Percentage out of 300 maximum |
| `grade = calculate_grade(percentage)` | Calls the grade function |
| f-string block | Prints the neatly formatted result card |

### 6. Dry Run

**Input:** m1 = 85, m2 = 76, m3 = 90

| Step | Calculation | Value |
|---|---|---|
| total | 85 + 76 + 90 | 251 |
| percentage | (251 / 300) × 100 | 83.67% |
| grade check | 83.67 ≥ 80 → A | **A** |

**Input:** m1 = 55, m2 = 48, m3 = 60

| Step | Calculation | Value |
|---|---|---|
| total | 55 + 48 + 60 | 163 |
| percentage | (163 / 300) × 100 | 54.33% |
| grade check | 54.33 ≥ 40 → D | **D** |

### 7. Sample Input / Output

```
Enter marks obtained in three subjects (each out of 100):
Subject 1: 85
Subject 2: 76
Subject 3: 90

----------- RESULT -----------
Total Marks    : 251.00 / 300
Percentage     : 83.67 %
Grade          : A
-------------------------------
```

### 8. Edge Cases Table

| Marks (m1,m2,m3) | Total | Percentage | Grade | Remark |
|---|---|---|---|---|
| 85, 76, 90 | 251 | 83.67 | A | Normal |
| 70, 72, 71 | 213 | 71.00 | B | Just above 70 |
| 60, 62, 61 | 183 | 61.00 | C | Just above 60 |
| 45, 42, 48 | 135 | 45.00 | D | Between 40–59 |
| 20, 30, 25 | 75 | 25.00 | E | Below 40 |
| 80, 80, 80 | 240 | 80.00 | A | Exactly 80 → A |
| 0, 0, 0 | 0 | 0.00 | E | Minimum |
| 100, 100, 100 | 300 | 100.00 | A | Maximum |
| Decimal marks (85.5, 79.0, 92.5) | 257.0 | 85.67 | A | Decimals handled by `float()` |

### 9. Common Mistakes to Avoid

1. **Wrong order in the grade chain** — if you check `>= 60` before `>= 70`, a 75% student would get grade C. Always check **highest band first**.
2. **Dividing by the wrong maximum** — if each subject is out of 100, the maximum is **300**, not 100 or 3. Percentage = `(total/300)*100`.
3. **Using `int()` instead of `float()`** — loses decimal marks (85.5 → 85).
4. **Not rounding** — use `:.2f` for clean output.
5. **Missing the `else` for E** — every percentage must map to a grade.
6. **Boundary confusion** — the criteria are "≥80 → A", "≥70 → B", etc. The `elif` chain handles the boundaries correctly because of the ordering.

### 10. Alternative Versions

**Grade logic as one line (conditional expression / `bisect` approach):**
```python
def calculate_grade(percentage):
    if percentage >= 80: return 'A'
    if percentage >= 70: return 'B'
    if percentage >= 60: return 'C'
    if percentage >= 40: return 'D'
    return 'E'
```

**Using `bisect` (compact, advanced):**
```python
import bisect

def calculate_grade(percentage):
    boundaries = [40, 60, 70, 80]          # ascending cut-off points
    grades = ['E', 'D', 'C', 'B', 'A']     # matching grades
    return grades[bisect.bisect_right(boundaries, percentage)]

print(calculate_grade(83.67))   # A
```

**Without functions (if functions are not required):**
```python
m1 = float(input("Subject 1: "))
m2 = float(input("Subject 2: "))
m3 = float(input("Subject 3: "))

total = m1 + m2 + m3
p = (total / 300) * 100

if p >= 80:
    grade = 'A'
elif p >= 70:
    grade = 'B'
elif p >= 60:
    grade = 'C'
elif p >= 40:
    grade = 'D'
else:
    grade = 'E'

print(f"Total = {total}, Percentage = {p:.2f}%, Grade = {grade}")
```

### 11. Brief Result Table (for the lab record)

| Subject 1 | Subject 2 | Subject 3 | Total | Percentage | Grade |
|---|---|---|---|---|---|
| 85 | 76 | 90 | 251 | 83.67% | A |
| 70 | 72 | 71 | 213 | 71.00% | B |
| 60 | 62 | 61 | 183 | 61.00% | C |
| 45 | 42 | 48 | 135 | 45.00% | D |
| 20 | 30 | 25 | 75 | 25.00% | E |

---
