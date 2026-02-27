# snake-whispers

A collection Python tips that quietly improve your code while pretending nothing happened.
No courses. No lectures. No 3-hour tutorials explaining `for` loops.

Just quiet, practical nudges toward writing cleaner, more Pythonic code.

## What is this?

**snake-whispers** is a collection of short Python programming tips (the kind you normally
learn after reading a lot of code, fixing subtle bugs, or being gently corrected in a code
review).

Each entry is:

* short
* practical
* opinionated (but survivable)
* immediately usable

Think of it as Python advice that leans over your shoulder and says:

> “There is a better way… but I’ll let you discover it.”

## What this is NOT

* ❌ Not a Python course
* ❌ Not beginner lessons
* ❌ Not academic explanations
* ❌ Not 40 screenshots and motivational quotes

If you need fundamentals, this repo will confuse you politely.

## Structure

Each tip lives in its own file:

```
dropbox/
  use_enumerate.md
  pathlib_over_os_path.md
  truthy_checks.md
```

Typical format:

* Problem
* Common approach
* Better Pythonic approach
* Why it matters
* Tiny example

Short enough to read during a coffee refill.

## Example Whisper

```python
# instead of
for i in range(len(items)):
    print(i, items[i])

# try
for i, item in enumerate(items):
    print(i, item)
```

Less noise. Same power. More Python.

## License

Use freely. Improve responsibly.
May cause sudden refactoring urges.
