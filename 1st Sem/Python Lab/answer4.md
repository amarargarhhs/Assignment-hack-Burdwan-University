# 4. WAP to Read n Integers and Display Them as a Histogram

### 1. Problem Understanding

Write a program that:
1. Reads **n** from the user.
2. Reads **n integer values**.
3. Displays them as a **histogram** — a chart of vertical bars where each bar's height represents one value.

The histogram can be drawn **two ways**:
- **Text histogram** (pure Python — prints `*` bars in the console), which always works anywhere.
- **Graphical histogram** (using `matplotlib` — a proper bar chart window).

We will implement **both**, with the graphical version as the main answer since the lab is "Visual Python".

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `matplotlib.pyplot` | Python's plotting library for graphs and charts |
| `plt.bar(x, values)` | Draws vertical bars for each value |
| `plt.xlabel/ylabel/title` | Chart labels |
| `plt.show()` | Displays the graph window |
| `plt.grid()` | Adds grid lines for readability |
| List comprehension | Compact way to build the value list |
| User-defined function | The histogram logic wrapped in a function |

> **Installation:** `pip install matplotlib`

### 3. Algorithm (Step-by-Step)

1. Start.
2. Read `n`.
3. Read `n` integers into a list `values`.
4. **Graphical histogram:**
   - `plt.bar(range(1, n+1), values)` — bar i has height values[i-1].
   - Label axes, add title and grid.
   - `plt.show()`.
5. **Text histogram (optional):**
   - For each value, print a row of `*` stars of length = value.
6. Stop.

### 4. The Python Program (Source Code)

```python
# Histogram of n integers

import matplotlib.pyplot as plt

def read_values(n):
    """Read n integers from the user and return them as a list."""
    values = []
    print(f"Enter {n} integers:")
    for i in range(n):
        v = int(input(f"Value {i+1}: "))
        values.append(v)
    return values

def draw_text_histogram(values):
    """Print a console histogram of '*' bars."""
    print("\n--- Text Histogram ---")
    for i, v in enumerate(values, start=1):
        print(f"{i:2d} | {'*' * v}")

def draw_graph_histogram(values):
    """Draw a graphical histogram using matplotlib."""
    n = len(values)
    x = range(1, n + 1)          # 1, 2, 3, ..., n

    plt.bar(x, values, color='skyblue', edgecolor='black')
    plt.xlabel("Index / Category")
    plt.ylabel("Value")
    plt.title("Histogram of n Integers")
    plt.grid(axis='y', linestyle='--', alpha=0.6)
    plt.show()

def main():
    n = int(input("How many integers? "))
    values = read_values(n)

    draw_text_histogram(values)      # console output
    draw_graph_histogram(values)     # graphical window

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `import matplotlib.pyplot as plt` | Imports the plotting module under the short alias `plt` |
| `def read_values(n):` | Function that collects n integers into a list |
| `values.append(v)` | Adds each entered value to the list |
| `for i, v in enumerate(values, start=1):` | Loops with index `i` starting at 1 and value `v` |
| `'*' * v` | Repeats the `*` character `v` times → text bar |
| `plt.bar(x, values, ...)` | Draws bars: `x` positions 1..n with heights `values` |
| `plt.xlabel("Index / Category")` | Label for the x-axis |
| `plt.grid(axis='y', linestyle='--', alpha=0.6)` | Dashed, semi-transparent horizontal grid lines |
| `plt.show()` | Opens the graph window (blocks until closed) |

### 6. Dry Run

**Input:** n = 5, values = [3, 5, 1, 4, 2]

**Text histogram output:**
```
--- Text Histogram ---
 1 | ***
 2 | *****
 3 | *
 4 | ****
 5 | **
```

**Graphical:** five bars of heights 3, 5, 1, 4, 2 appear in a window.

### 7. Sample Input / Output

```
How many integers? 4
Enter 4 integers:
Value 1: 2
Value 2: 7
Value 3: 4
Value 4: 5

--- Text Histogram ---
 1 | **
 2 | *******
 3 | ****
 4 | *****
```

Then a matplotlib window opens with the corresponding bar chart.

### 8. Edge Cases Table

| Input | Behaviour |
|---|---|
| n = 1 | Single bar |
| Negative values | Bar height negative → text histogram prints nothing, graph shows bar below axis |
| n = 0 | Empty chart / no rows — consider validating |
| Very large values | Bar extends proportionally; text row may be very long |
| Non-integer input | `int(input(...))` raises `ValueError` — use `try/except` to be robust |

### 9. Common Mistakes to Avoid

1. **Using `plt.hist()` instead of `plt.bar()`** — `plt.hist()` computes a frequency distribution of data; `plt.bar()` draws one bar per given value, which is what the question asks.
2. **Forgetting `plt.show()`** — the graph never appears.
3. **Forgetting to install matplotlib** — `ModuleNotFoundError`. Install with `pip install matplotlib`.
4. **Integer parse error** — wrap input in `try/except` for invalid entries.
5. **X positions off by one** — `range(1, n+1)` labels bars 1..n, matching the list positions.
6. **Not using functions** — the lab requires user-defined functions where possible.

### 10. Alternative Version — Single Function, Pure Console Histogram

```python
def histogram():
    n = int(input("How many integers? "))
    nums = []
    for i in range(n):
        nums.append(int(input(f"Value {i+1}: ")))

    print("\nHistogram:")
    for num in nums:
        print("*" * num)

histogram()
```

### 11. Brief Result Table (for the lab record)

| n | Values | Text histogram |
|---|---|---|
| 5 | 3, 5, 1, 4, 2 | `***`, `*****`, `*`, `****`, `**` |
| 4 | 2, 7, 4, 5 | `**`, `*******`, `****`, `*****` |
| 3 | 1, 2, 3 | `*`, `**`, `***` |

---
