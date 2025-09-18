# AI-Assisted Code Refactor & Testing — Live Demo (15 minutes)

This README is a step-by-step guide you can follow to run the 15-minute live demo showing how GitHub Copilot (or similar AI) can help generate, refactor, and test code. It includes everything from creating the repository to running tests, injecting small bugs, and speaker notes.

---

## Goal
Show how AI can:
1. Generate a deliberately poor implementation.
2. Refactor the code into a robust version.
3. Generate unit tests.
4. Catch regressions by running tests after injecting small bugs.

---

## What you will demonstrate
- A Python function `word_stats(text)` that returns a dict with:
  - `words`: number of words
  - `unique`: number of unique words
  - `avg_len`: average word length
- The demo shows three stages: **poor code → refactor → tests** and then **inject/fix bugs**.

---

## Prerequisites
- Python 3.8+ installed on your machine.
- Visual Studio Code (recommended).
- GitHub Copilot extension in VS Code (optional — you can paste prompts manually).
- `pytest` installed in your virtualenv.
```pip install pytest
```

---

# Step-by-step demo script 

Step 0 — Opening lines (0:00–0:30)

"Hello everyone! Today we’ll explore how AI can help us write, refactor, and test code. The star of our demo is a Python function that calculates word statistics: number of words, number of unique words, and average word length. We’ll begin with a poorly written version, fix it through refactoring, and then add tests to catch errors."

Short definition:
"A function is simply a block of code that performs a task—in this case, analyzing text."

# Step 1 — Generate the POOR implementation (2–3 min)

### Prompt 1:
``` Write a deliberately poor Python function called `word_stats(text)` that returns a dictionary with keys:
- "words" = number of words,
- "unique" = number of unique words,
- "avg_len" = average word length.

But make it intentionally flawed:
- Split the text only on a single space character " ",
- Keep uniqueness case-sensitive (so 'Hello' and 'hello' count separately),
- Do not strip punctuation,
- Use integer division (//) for average length,
- If text is None, return {"words": 0, "unique": 0, "avg_len": 0}.
Add a short docstring saying it’s a poor implementation.
```
- This function counts words, unique words, and average word length. But it’s poorly written on purpose:
   - It only splits by spaces, so punctuation sticks to words.
   - It treats ‘Hello’ and ‘hello’ as different words.
   - It uses integer division, so decimal averages are lost.
   - And for None, it returns avg_len as 0 instead of 0.0
- This is how a poor implementation looks. It works, but it’s fragile and inconsistent. In real life, code like this can cause subtle bugs

# Step 2 - Refactor with Copilot

### Prompt 2:
``` Refactor this function to be robust:
- normalize to lowercase,
- extract words using regex so punctuation doesn't attach to words,
- ensure average word length is a float and rounded to 2 decimals,
- return consistent types (avg_len float),
- add a short docstring and type hints.
Keep the function signature word_stats(text: str) -> dict.
```

- Refactoring means improving code structure without changing its output. It makes code cleaner, more robust, and easier to maintain.
- Now, here’s what’s changed:
  - Words are extracted using regex, so punctuation is ignored.
  - Everything is converted to lowercase, so uniqueness works correctly.
  - Average word length uses float division, rounded to 2 decimals.
  - And None or empty input safely return zeroes.
- This is refactoring—making the same function behave correctly and more reliably, while the input-output meaning stays the same.

# Step 3: Generate Test Cases using copilot and validate

### Prompt 3:
``` Generate pytest unit tests that cover:
A sentence with punctuation and repeated words (case differences),
A case where average is non-integer (to catch integer division),
Case-insensitive uniqueness,
Empty string and None
```

- These tests cover multiple scenarios:
  - Handling punctuation and repeated words.
  - Making sure averages don’t get rounded off incorrectly.
  - Checking case-insensitive uniqueness.
  - And safe handling of empty or None input.

#### Run:
``` pytest test_text_utils.py
```
- All Pass

## Introduce Bug
- Now let’s deliberately introduce a tiny bug, just to see how tests react.

### BUG: using floor division and adding 0.1 to force incorrect avg for some inputs
``` avg_len = round(total_len // len(words) + 0.1, 2) if words else 0.0
```

- Logically, it looks harmless. Let’s see if our tests notice.
👉 Run tests → one test fails, the rest pass.

- That’s how even a tiny type mismatch is caught by tests immediately.
- Finally, let’s fix the bug back  and rerun the tests.
``` avg_len = round(total_len / len(words), 2) if words else 0.0
```

### BUG: accidentally decrement unique count by 1 when there are words

``` unique = len(set(words)) - 1 if words else 0
```

- Correct: 
``` unique = len(set(words))
```

## Conclusion
So today we saw how Copilot can:
- Generate even poor code for learning,
- Refactor it into robust code,
- Generate tests to validate behavior,
- And how tests instantly catch even tiny bugs.

That’s the power of AI-assisted development combined with testing.”







