# 6. WAP to Display a Graph of Pulse Rate (y) Against Height (x)

### 1. Problem Understanding

A researcher records the **pulse rate** (heartbeats per minute) of a person at different **heights** (metres above sea level, or simply the height of an activity). The task is to plot a graph with:

- **X-axis → height (in metres)**
- **Y-axis → pulse rate (beats per minute)**

Since no actual data table is provided in the question, the program reads the **(height, pulse rate)** pairs from the user (or uses sample data) and draws the graph. A **scatter/line plot** is the natural choice.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `matplotlib.pyplot` | Plotting library |
| Two parallel lists | `heights` and `pulse_rates` read together |
| `plt.plot(x, y, 'o-')` | Line + markers plot ('o' marker, '-' line) |
| `plt.xlabel/ylabel` | Axis labels ("Height (m)", "Pulse Rate (bpm)") |
| `plt.title` | Graph title |
| `plt.show()` | Displays the graph |

### 3. Algorithm (Step-by-Step)

1. Import `matplotlib.pyplot`.
2. Read the number of observations `n`.
3. Loop `n` times, reading a height and its pulse rate; store in two lists.
4. `plt.plot(heights, pulse_rates, 'o-')`.
5. Label axes, add title and grid.
6. `plt.show()`.

### 4. The Python Program (Source Code)

```python
# Pulse rate vs Height graph

import matplotlib.pyplot as plt

def read_data(n):
    """Read n (height, pulse) pairs and return two lists."""
    heights = []
    pulse = []
    print("Enter height (m) and pulse rate (bpm):")
    for i in range(n):
        h = float(input(f"  Height {i+1}: "))
        p = float(input(f"  Pulse  {i+1}: "))
        heights.append(h)
        pulse.append(p)
    return heights, pulse

def plot_graph(heights, pulse):
    """Plot pulse rate against height."""
    plt.plot(heights, pulse, 'o-', color='crimson',
             linewidth=2, markersize=6)
    plt.xlabel("Height (metres)")
    plt.ylabel("Pulse Rate (beats per minute)")
    plt.title("Pulse Rate vs Height")
    plt.grid(True, linestyle='--', alpha=0.6)
    plt.show()

def main():
    n = int(input("How many observations? "))
    heights, pulse = read_data(n)
    plot_graph(heights, pulse)

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `def read_data(n):` | User-defined function collecting the data pairs |
| `h = float(input(...))` / `p = float(input(...))` | Reads one height and its pulse rate |
| `heights.append(h)`, `pulse.append(p)` | Store in parallel lists |
| `plt.plot(heights, pulse, 'o-', ...)` | Line joining circle markers — the graph |
| `'o-'` | Format string: `o` = round markers, `-` = solid line |
| `color='crimson'`, `linewidth=2`, `markersize=6` | Styling for visibility |
| `plt.xlabel("Height (metres)")` | Correct unit on the x-axis |
| `plt.ylabel("Pulse Rate (beats per minute)")` | Correct unit on the y-axis |
| `plt.grid(...)` | Dashed light grid for readability |

### 6. Dry Run (Sample Data)

**Input:** n = 6

| Height (m) | Pulse rate (bpm) |
|---|---|
| 0 | 72 |
| 1 | 74 |
| 2 | 77 |
| 3 | 81 |
| 4 | 86 |
| 5 | 92 |

The graph shows a gently rising curve — pulse rate increasing with height, which matches the expectation that effort (going uphill) raises the heart rate.

### 7. Sample Input / Output

```
How many observations? 3
Enter height (m) and pulse rate (bpm):
  Height 1: 0
  Pulse 1: 70
  Height 2: 2.5
  Pulse 2: 80
  Height 3: 5
  Pulse 3: 95
```

A window opens with a crimson line + markers connecting (0,70), (2.5,80), (5,95), titled **"Pulse Rate vs Height"**.

### 8. Edge Cases & Notes

| Case | Behaviour / Note |
|---|---|
| Single observation | One isolated dot on the graph |
| Two observations | A straight line between them |
| Heights out of order | `plt.plot` connects points in the order given; use `sorted` if a smooth x-order is wanted |
| Decimal values | Handled — inputs use `float()` |
| Negative height / pulse | Plotted as-is; physically unusual but drawn |

### 9. Common Mistakes to Avoid

1. **Forgetting `plt.show()`** — no graph window appears.
2. **Mixing up axes** — question says pulse rate on **Y** and height on **X**; put `heights` first in `plt.plot`.
3. **Using `plt.scatter` only** — fine, but a line (`plt.plot` with `'o-'`) makes the trend clearer.
4. **Misaligned lists** — always append height and pulse in the same loop iteration so indexes match.
5. **No units in labels** — examiners expect "(metres)" and "(beats per minute)".

### 10. Alternative Version — Scatter Plot with Sample Data

```python
import matplotlib.pyplot as plt

# sample readings
heights  = [0, 1, 2, 3, 4, 5]
pulse    = [72, 74, 77, 81, 86, 92]

plt.scatter(heights, pulse, color='crimson', s=50)
plt.plot(heights, pulse, color='crimson', linewidth=1.5)
plt.xlabel("Height (metres)")
plt.ylabel("Pulse Rate (beats per minute)")
plt.title("Pulse Rate vs Height")
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()
```

### 11. Brief Result Table (for the lab record)

| Height (m) | Pulse rate (bpm) |
|---|---|
| 0 | 72 |
| 1 | 74 |
| 2 | 77 |
| 3 | 81 |
| 4 | 86 |
| 5 | 92 |

**Observation:** pulse rate rises with height — as physical effort (climbing) increases, the heart beats faster to supply oxygen to muscles.

---
