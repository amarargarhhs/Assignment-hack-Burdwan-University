# 7. WAP to Display the Curve m = 60/(t + 2) — Mass Disintegration

### 1. Problem Understanding

A radioactive substance decays with time. Its remaining **mass m** (grams) is given by the rational function:

> **m = 60 / (t + 2)**

where **t** is the time (in suitable units, e.g. minutes or hours). The task is to plot **t (x-axis)** against **m (y-axis)**.

**Physics of the formula:**
- At **t = 0**: m = 60 / 2 = **30 g** (initial amount).
- As **t → ∞**: m → **0 g** (mass decays away asymptotically).
- The curve is a **rectangular hyperbola / inverse function**, showing exponential-like decay (this particular one is rational decay).

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `numpy` (`np`) | `np.linspace` for a smooth t range |
| `m = 60 / (t + 2)` | Vectorized evaluation of the rational formula |
| `matplotlib.pyplot` | Plotting the decay curve |
| `plt.plot(t, m, ...)` | The t vs m graph |
| `plt.xlabel("Time t")` / `plt.ylabel("Mass m")` | Axis labels |
| Asymptotic behaviour | m gets smaller but never negative |

### 3. Algorithm (Step-by-Step)

1. Import `numpy as np` and `matplotlib.pyplot as plt`.
2. Create `t = np.linspace(0, 60, 300)` — time from 0 to 60 units.
3. Compute `m = 60 / (t + 2)` for every t at once.
4. Plot `plt.plot(t, m)`.
5. Label axes, add title and grid.
6. `plt.show()`.

### 4. The Python Program (Source Code)

```python
# Mass disintegration: m = 60 / (t + 2)

import numpy as np
import matplotlib.pyplot as plt

def mass(t):
    """Return the mass m for a given time t."""
    return 60 / (t + 2)

def main():
    t = np.linspace(0, 60, 300)          # 300 points from t=0 to t=60
    m = mass(t)                           # mass at every t

    plt.plot(t, m, color='purple', linewidth=2)
    plt.xlabel("Time (t)")
    plt.ylabel("Mass m(t) = 60/(t+2) (grams)")
    plt.title("Mass Disintegration Curve")
    plt.grid(True, linestyle='--', alpha=0.6)
    plt.show()

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `import numpy as np` | Enables vectorized math on arrays |
| `def mass(t): return 60 / (t + 2)` | User-defined function — the disintegration law |
| `np.linspace(0, 60, 300)` | 300 evenly spaced times from 0 to 60 |
| `m = mass(t)` | Computes all masses in one call (works because `t` is an array) |
| `plt.plot(t, m, color='purple', linewidth=2)` | Draws the smooth decay curve |
| `plt.grid(...)` | Dashed grid improves readability |
| `plt.show()` | Opens the graph window |

### 6. Dry Run (Analytical Values)

| t | t + 2 | m = 60/(t+2) |
|---|---|---|
| 0 | 2 | 30.00 |
| 1 | 3 | 20.00 |
| 2 | 4 | 15.00 |
| 3 | 5 | 12.00 |
| 4 | 6 | 10.00 |
| 8 | 10 | 6.00 |
| 18 | 20 | 3.00 |
| 28 | 30 | 2.00 |
| 58 | 60 | 1.00 |
| 298 | 300 | 0.20 |

The curve starts at (0, 30), falls steeply at first, then flattens towards zero.

### 7. Sample Output

A window titled **"Mass Disintegration Curve"** shows a purple curve:
- Starting at m = 30 when t = 0,
- Dropping quickly for small t,
- Levelling off towards the x-axis (m → 0) as t grows large.

### 8. Edge Cases & Notes

| Case | Behaviour / Note |
|---|---|
| t = −2 | **Division by zero** — m is undefined; so the graph domain must stay above −2 |
| t < −2 | m becomes negative — physically meaningless for mass |
| t = 0 | m = 30 (initial mass) |
| t very large | m → 0 but never exactly 0 (asymptote at m = 0) |
| Using t = −2 accidentally | NumPy prints a warning and produces `inf` — avoid the point |

### 9. Common Mistakes to Avoid

1. **Starting the t-range at a value where t + 2 = 0** — causes division by zero. Start at t = 0 or above.
2. **Forgetting the parentheses** — `60 / t + 2` means `(60/t) + 2`, which is a **different curve**. The formula must be `60 / (t + 2)`.
3. **Too few points** — the curve looks jagged; use 200–400 points.
4. **Wrong axis labels** — t on x-axis, m on y-axis, per the question.
5. **Not using a function** — wrap the formula in `def mass(t)` as the lab requires.

### 10. Alternative Version — Console Table of Values

```python
# Print a table instead of (or in addition to) the graph

print("t\tm = 60/(t+2)")
for t in range(0, 21):
    m = 60 / (t + 2)
    print(f"{t}\t{m:.2f}")
```

### 11. Brief Result Table (for the lab record)

| t | 0 | 1 | 2 | 3 | 4 | 8 | 18 |
|---|---|---|---|---|---|---|---|
| **m** | 30.00 | 20.00 | 15.00 | 12.00 | 10.00 | 6.00 | 3.00 |

**Observation:** mass decays hyperbolically; half the initial value (15 g) is reached at t = 2.

---
