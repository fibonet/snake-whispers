## 5 Uncommon Python Features ([Source](https://youtu.be/sQ1Q96-Vhjk?si=AyUm2KWoNSULLcfa))

### 1. Slice Objects

```python
numbers: list[int] = list(range(1,11))
text: str = 'Hello, world'

rev: slice = slice(None, None, -1)
f_five: slice = slice(None, 5)

print(numbers[rev])
# [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
print(text[rev])
# dlrow ,olleH

print(numbers[f_five])
# [1, 2, 3, 4, 5]
print(text[f_five])
# Hello
```

---

### 2. Set Operations

```python
set_a: set[int] = {1, 2, 3, 4, 5}
set_b: set[int] = {4, 5, 6, 7, 8}

# Union - Combines all elements from both sets.
print(set_a | set_b)
# {1, 2, 3, 4, 5, 6, 7, 8}

# Difference - Keeps elements that are in the left set only.
print(set_a - set_b)
# {1, 2, 3}
print(set_b - set_a)
# {8, 6, 7}

# Intersection - Keeps only elements common to both sets.
print(set_a & set_b)
# {4, 5}

# Symmetric Difference - Keeps elements that are in exactly one of the sets.
print(set_a ^ set_b)
# {1, 2, 3, 6, 7, 8}
```

---

### 3. Format

```python
from typing import Any

class Book:
    def __init__(self, title: str, pages: int) -> None:
        self.title = title
        self.pages = pages

    def __format__(self, format_spec: Any) -> str:
        match format_spec:
            case 'time':
                return f'{self.pages / 60:.2f}h'
            case 'caps':
                return self.title.upper()
            case _:
                raise ValueError(f'Unknown specifier for Book()')

def main() -> None:
    hairy_potter: Book = Book('Very Hairy Potter', 300)
    python_daily: Book = Book('Python Daily', 20)

    print(f'{hairy_potter:caps}') # VERY HAIRY POTTER
    print(f'Read time {hairy_potter:time}') # Read time 5.00h

    print(f'{python_daily:caps}') # PYTHON DAILY
    print(f'Read time {python_daily:time}') # Read time 0.33h

if __name__ == '__main__':
    main()
```
---

### 4. Walrus operator

#### Example
```python
users: dict[int, str] = {0: 'Bob', 1: 'Mario'}
```

Without walrus operator:
```python
user: str | None = users.get(1)

if user:
    print(f'User: {user} exists!')
else:
    print('No user found...')
```

With walrus operator:
```python
if user := users.get(0):
    print(f'User: {user} exists!')
else:
    print('No user found...')
```

#### Another example

```python
def get_info(text: str) -> dict:
    return {'words':(words := text.split()),
            'word_count': len(words),
            'character_count': len(''.join(words))}


print(get_info('Bob'))
# {'words': ['Bob'], 'word_count': 1, 'character_count': 3}
print(get_info('Hello, Bob'))
# {'words': ['Hello,', 'Bob'], 'word_count': 2, 'character_count': 9}
print(get_info('My name is Bob!'))
# {'words': ['My', 'name', 'is', 'Bob!'], 'word_count': 4, 'character_count': 12}
```

---

### 5. Currying

```python
from typing import Callable

def multiply_setup(a: float) -> Callable:
    def multiply(b: float) -> float:
        return a * b
    return multiply

double: Callable = multiply_setup(2)
triple: Callable = multiply_setup(3)

print(double(5))   # 10
print(double(3))   # 6
print(double(100)) # 200

print(triple(5))   # 15
print(triple(3))   # 9
print(triple(100)) # 300
```
