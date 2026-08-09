# 5. WAP to Display Sine, Cosine, Polynomial and Exponential Curves

### 1. Problem Understanding

Write a program that plots **four mathematical curves** on one set of axes:

| Curve | Formula (domain used) |
|---|---|
| **Sine** | y = sin(x), for x in [−2π, 2π] |
| **Cosine** | y = cos(x), for x in [−2π, 2π] |
| **Polynomial** | y = x² (or x² − x + 1), for x in [−3, 3] |
| **Exponential** | y = eˣ, for x in [−2, 3] (typical) |

All curves are drawn on the **same graph** using `matplotlib`'s `numpy` support.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `numpy` (`np`) | Creates arrays of x values and vectorized math (sin, cos, exp) |
| `np.linspace(a, b, num)` | Generates `num` evenly spaced points between a and b |
| `np.sin` / `np.cos` / `np.exp` | Element-wise math on the whole array |
| `plt.plot(x, y, label=...)` | Draws a curve and tags it for the legend |
| `plt.legend()` | Shows which colour = which curve |
| `plt.axhline(0)` / `plt.axvline(0)` | Axis lines through the origin |

> **Installation:** `pip install matplotlib numpy`

### 3. Algorithm (Step-by-Step)

1. Import `numpy as np` and `matplotlib.pyplot as plt`.
2. Create the x-range for the **trig curves**: `np.linspace(-2*np.pi, 2*np.pi, 400)`.
3. Compute `y1 = np.sin(x1)`, `y2 = np.cos(x1)`.
4. Create the x-range for the **polynomial**: `np.linspace(-3, 3, 200)`; compute `y3 = x2**2`.
5. Create the x-range for the **exponential**: `np.linspace(-2, 3, 200)`; compute `y4 = np.exp(x3)`.
6. Plot all four curves with `label=`.
7. Add legend, axis lines, grid, title.
8. `plt.show()`.

### 4. The Python Program (Source Code)

```python
# Plot sine, cosine, polynomial and exponential curves together

import numpy as np
import matplotlib.pyplot as plt

def plot_sine_cosine():
    x1 = np.linspace(-2 * np.pi, 2 * np.pi, 400)
    y_sin = np.sin(x1)
    y_cos = np.cos(x1)
    plt.plot(x1, y_sin, label='sin(x)', color='blue')
    plt.plot(x1, y_cos, label='cos(x)', color='red')

def plot_polynomial():
    x2 = np.linspace(-3, 3, 200)
    y_poly = x2 ** 2
    plt.plot(x2, y_poly, label='x^2', color='green')

def plot_exponential():
    x3 = np.linspace(-2, 3, 200)
    y_exp = np.exp(x3)
    plt.plot(x3, y_exp, label='e^x', color='orange')

def main():
    plot_sine_cosine()
    plot_polynomial()
    plot_exponential()

    plt.axhline(0, color='black', linewidth=0.8)
    plt.axvline(0, color='black', linewidth=0.8)
    plt.grid(True, linestyle='--', alpha=0.6)
    plt.title("Sine, Cosine, Polynomial and Exponential Curves")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.legend()
    plt.show()

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `import numpy as np` | Array math library (needed for sin/cos/exp over many points) |
| `import matplotlib.pyplot as plt` | Plotting library |
| `np.linspace(-2*np.pi, 2*np.pi, 400)` | 400 points from −6.28… to +6.28… for smooth trig curves |
| `np.sin(x1)` / `np.cos(x1)` | Computes the full sine/cosine curve in one vectorized call |
| `x2 ** 2` | Element-wise squaring → the polynomial y = x² |
| `np.exp(x3)` | Element-wise eˣ |
| `plt.plot(x, y, label='sin(x)', color='blue')` | Draws the curve; `label` feeds the legend, `color` picks the colour |
| `plt.axhline(0)` / `plt.axvline(0)` | Thin black lines along the x and y axes for reference |
| `plt.grid(True, linestyle='--', alpha=0.6)` | Dashed light grid |
| `plt.legend()` | Places the box that maps colours → names |
| `plt.show()` | Displays the combined plot |

### 6. Dry Run (Numerical Check of Curve Values)

| x | sin(x) | cos(x) | x² | eˣ |
|---|---|---|---|---|
| 0 | 0 | 1 | 0 | 1 |
| π/2 (1.5708) | 1 | ≈0 | — | — |
| π | ≈0 | −1 | — | — |
| −3 | — | — | 9 | ≈0.05 |
| 2 | — | — | 4 | ≈7.39 |

The plot shows: sine and cosine oscillating between −1 and 1; the parabola opening upward; the exponential rising steeply.

### 7. Sample Output

A single window titled **"Sine, Cosine, Polynomial and Exponential Curves"** shows:
- Blue wavy sine curve,
- Red wavy cosine curve,
- Green U-shaped parabola,
- Orange steeply rising exponential,
- Legend, grid, and axis lines.

### 8. Edge Cases & Notes

| Case | Behaviour / Note |
|---|---|
| Very small range for exponential (e.g. −2..3) | Keeps y within a readable range; e³ ≈ 20 still fits with polynomial |
| Large polynomial degree | Curve grows fast, may flatten the others — use degree 2 or 3 |
| `x**2` vs `x^2` | Python uses `**` for power; `^` is XOR and is a common bug |
| Old Python without numpy | Install with `pip install numpy` |

### 9. Common Mistakes to Avoid

1. **Using `x^2`** — that's bitwise XOR in Python, not squaring. Use `x**2`.
2. **Not enough points** — too few points make the sine/cosine look jagged; 400 is smooth.
3. **Only importing `matplotlib`** but not `numpy` — you can't vectorize sin/cos/exp over many x-values easily.
4. **No legend/labels** — the marker of a good graph is clear labels and a legend.
5. **Plotting on different scales** — exponential (y up to ~20) and sine (y between −1 and 1) share the axis; that's fine here, just be aware the small curves look flat.

### 10. Alternative Version — Separate Subplots (one per curve)

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-2*np.pi, 2*np.pi, 400)

fig, ax = plt.subplots(2, 2, figsize=(9, 7))

ax[0,0].plot(x, np.sin(x), 'b'); ax[0,0].set_title("y = sin(x)")
ax[0,1].plot(x, np.cos(x), 'r'); ax[0,1].set_title("y = cos(x)")

xp = np.linspace(-3, 3, 200)
ax[1,0].plot(xp, xp**2, 'g');  ax[1,0].set_title("y = x^2")
ax[1,1].plot(xp, np.exp(xp), 'orange'); ax[1,1].set_title("y = e^x")

plt.tight_layout()
plt.show()
```

### 11. Brief Result Table (for the lab record)

| Curve | Code | x range used | Shape |
|---|---|---|---|
| sine | `np.sin(x)` | −2π to 2π | Wave between −1 and 1 |
| cosine | `np.cos(x)` | −2π to 2π | Wave between −1 and 1 (shifted) |
| polynomial | `x**2` | −3 to 3 | Upward parabola |
| exponential | `np.exp(x)` | −2 to 3 | Steep rising curve |

---
