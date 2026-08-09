# 1. Menu-Driven Program to Convert Temperature (Fahrenheit ↔ Celsius)

### 1. Problem Understanding

Write a **menu-driven Python program** that converts temperature between two scales, depending on the user's choice:

- **Option 1:** Fahrenheit → Celsius
- **Option 2:** Celsius → Fahrenheit

**The conversion formulas:**
```
C = (F - 32) * 5/9          Fahrenheit to Celsius
F = (C * 9/5) + 32          Celsius to Fahrenheit
```

A menu is displayed, the user picks an option, enters a temperature value, and the converted value is printed. The menu should repeat until the user chooses to exit.

### 2. Key Concepts Used

| Concept | Explanation |
|---|---|
| `while True` loop | Keeps showing the menu until the user exits |
| `if/elif/else` | Handles each menu choice |
| `float()` | Reads temperature as a real number (decimal allowed) |
| Format strings (`f-string`) | Prints clean, formatted output |
| `break` | Exits the menu loop |

### 3. Algorithm (Step-by-Step)

1. Start.
2. Show the menu:
   - 1. Fahrenheit → Celsius
   - 2. Celsius → Fahrenheit
   - 3. Exit
3. Read the user's choice.
4. **If choice == 1:** read temperature in °F, convert with `C = (F-32)*5/9`, print result.
5. **If choice == 2:** read temperature in °C, convert with `F = (C*9/5)+32`, print result.
6. **If choice == 3:** exit the program.
7. Otherwise print "Invalid choice".
8. Repeat from step 2 until the user exits.

### 4. The Python Program (Source Code)

```python
# Temperature conversion: Fahrenheit <-> Celsius

def fahrenheit_to_celsius(f):
    """Convert Fahrenheit to Celsius."""
    return (f - 32) * 5 / 9

def celsius_to_fahrenheit(c):
    """Convert Celsius to Fahrenheit."""
    return (c * 9 / 5) + 32

def main():
    while True:
        print("\n----- TEMPERATURE CONVERTER -----")
        print("1. Fahrenheit to Celsius")
        print("2. Celsius to Fahrenheit")
        print("3. Exit")
        choice = input("Enter your choice (1/2/3): ")

        if choice == '1':
            f = float(input("Enter temperature in Fahrenheit: "))
            c = fahrenheit_to_celsius(f)
            print(f"{f} °F = {c:.2f} °C")

        elif choice == '2':
            c = float(input("Enter temperature in Celsius: "))
            f = celsius_to_fahrenheit(c)
            print(f"{c} °C = {f:.2f} °F")

        elif choice == '3':
            print("Exiting the program. Goodbye!")
            break

        else:
            print("Invalid choice! Please enter 1, 2 or 3.")

if __name__ == "__main__":
    main()
```

### 5. Line-by-Line Explanation

| Code | What it does |
|---|---|
| `def fahrenheit_to_celsius(f):` | User-defined function returning the °C value |
| `(f - 32) * 5 / 9` | The standard F→C formula |
| `def celsius_to_fahrenheit(c):` | User-defined function returning the °F value |
| `(c * 9 / 5) + 32` | The standard C→F formula |
| `while True:` | Infinite loop — menu repeats until broken out of |
| `input("Enter your choice...")` | Reads the choice as a **string** |
| `if choice == '1':` | Compares the string choice with '1' |
| `float(input(...))` | Reads the temperature as a float (accepts 98.6, 37.0, etc.) |
| `f"{f} °F = {c:.2f} °C"` | f-string; `:.2f` rounds the result to 2 decimal places |
| `elif choice == '3': ... break` | Exits the loop and ends the program |
| `if __name__ == "__main__":` | Standard Python idiom — runs `main()` only when the script is executed directly |

### 6. Dry Run

**Input:** choice = 1, F = 98.6

| Step | Operation | Value |
|---|---|---|
| 1 | `c = (98.6 - 32) * 5/9` | `(66.6 * 5)/9` = 37.0 |
| 2 | Output | `98.6 °F = 37.00 °C` ✓ (98.6 °F is normal body temperature = 37 °C) |

**Input:** choice = 2, C = 100

| Step | Operation | Value |
|---|---|---|
| 1 | `f = (100 * 9/5) + 32` | 180 + 32 = 212.0 |
| 2 | Output | `100 °C = 212.00 °F` ✓ (boiling point of water) |

### 7. Sample Input / Output

```
----- TEMPERATURE CONVERTER -----
1. Fahrenheit to Celsius
2. Celsius to Fahrenheit
3. Exit
Enter your choice (1/2/3): 1
Enter temperature in Fahrenheit: 32
32.0 °F = 0.00 °C

----- TEMPERATURE CONVERTER -----
1. Fahrenheit to Celsius
2. Celsius to Fahrenheit
3. Exit
Enter your choice (1/2/3): 2
Enter temperature in Celsius: 212
212.0 °C = 413.60 °F

Enter your choice (1/2/3): 3
Exiting the program. Goodbye!
```

### 8. Edge Cases Table

| Input | Result | Remark |
|---|---|---|
| F = 32 | 0 °C | Freezing point of water |
| F = 212 | 100 °C | Boiling point of water |
| F = 98.6 | 37.0 °C | Normal human body temperature |
| C = 0 | 32 °F | Freezing point |
| C = 100 | 212 °F | Boiling point |
| C = −40 | −40 °F | The only point where both scales are equal |
| Decimal input | Handled by `float()` | e.g., 98.6, 37.5 |
| Non-numeric input | `ValueError` crash | Fix: wrap in `try/except` (see alternative) |

### 9. Common Mistakes to Avoid

1. **Forgetting the menu loop** — the program runs once and stops; a menu-driven program must repeat.
2. **Comparing choice to `1` (int) instead of `'1'` (string)** — `input()` always returns a string; `choice == 1` is never True.
3. **Using `int()` for temperature** — loses decimals (98.6 becomes 98). Use `float()`.
4. **Integer division gotcha in Python 2** — `5/9` = 0 in Python 2. In Python 3 `5/9` is fine, but write `5/9` clearly. If using Python 2 add `from __future__ import division`.
5. **Not handling invalid menu choice** — the `else` branch is essential.
6. **Missing the `if __name__ == "__main__"` guard** — good practice; without it the code runs even when imported.

### 10. Alternative Versions

**With `try/except` (robust input handling):**
```python
def main():
    while True:
        print("\n1. Fahrenheit to Celsius")
        print("2. Celsius to Fahrenheit")
        print("3. Exit")
        choice = input("Enter your choice: ")

        if choice in ('1', '2'):
            try:
                val = float(input("Enter temperature: "))
            except ValueError:
                print("Invalid number! Please enter a valid temperature.")
                continue

            if choice == '1':
                print(f"{val} °F = {(val - 32) * 5/9:.2f} °C")
            else:
                print(f"{val} °C = {(val * 9/5) + 32:.2f} °F")

        elif choice == '3':
            print("Goodbye!")
            break
        else:
            print("Invalid choice!")
```

**Without functions (simplest, if functions are not required):**
```python
while True:
    print("\n1. F to C\n2. C to F\n3. Exit")
    ch = input("Choice: ")
    if ch == '1':
        f = float(input("F: "))
        print(f"Celsius = {(f - 32) * 5/9:.2f}")
    elif ch == '2':
        c = float(input("C: "))
        print(f"Fahrenheit = {c * 9/5 + 32:.2f}")
    elif ch == '3':
        break
    else:
        print("Invalid!")
```

### 11. Brief Result Table (for the lab record)

| Option | Input | Output |
|---|---|---|
| F → C | 32 °F | 0.00 °C |
| F → C | 212 °F | 100.00 °C |
| C → F | 100 °C | 212.00 °F |
| C → F | −40 °C | −40.00 °F |
| Exit | 3 | Program ends |

---
