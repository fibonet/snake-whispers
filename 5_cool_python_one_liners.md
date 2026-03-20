## 5 Cool Python One-Liners ([Source](https://youtu.be/kfZOrjVXSms?si=bqDHOfG7mQXvy1OK))

### 1. Reverse slice

```python
phrase: str = 'Hello, Bob!'

print(phrase[::-1]) # !boB ,olleH
```

### 2. Elvis operator

Instead of:

```python
def valid_length(user_input: str) -> str:
    if len(user_input) > 10:
        return "Yes case"
    else:
        return "No case"
```

You can do this:

```python
def valid_length(user_input: str) -> str:
    return 'Yes case' if len(user_input) > 10 else 'No case'
```

### 3. Flatten

```python
from typing import Callable, Any

flatten = lambda target: sum((flatten(sub) if isinstance(sub, list) else [sub] for sub in target), [])

nested_list: list[Any] = [1, [2, [3, 4], 5], [6, 7], 8, [[[[[[9, 10]]]]]]]

print(flatten(nested_list)) # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

With the new type syntax from Python 3.12:

```python
type: Flatten = Callable[[list], list]
flatten: Flatten = lambda target: sum((flatten(sub) if isinstance(sub, list) else [sub] for sub in target), [])
```

### 4. Password generator

```python
from secrets import choice
from string import ascii_letters, digits, punctuation
from typing import Callable

pass_gen: Callable[[int], str] = lambda x: ''.join(choice(ascii_letters + digits + punctuation) for _ in range(x))

def main():
    print(pass_gen(4))
    print(pass_gen(20))
    print(pass_gen(10))

if __name__ == "__main__":
    main()
```

### 5. Email extractor

```python
import re
from typing import Callable

type Email = Callable[[str], list[str | None]]
get_emails: Email = lambda text: re.findall(
    r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)


def main() -> None:
    with open('text.txt') as txt:
        sample_text: str = txt.read()

    print(get_emails(sample_text))


if __name__ == '__main__':
    main()
```