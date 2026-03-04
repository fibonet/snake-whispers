## 5 Good Python Habits ([Source](https://www.youtube.com/watch?v=I72uD8ED73U))

### 1. `if __name__ == '__main__'`

#### 🧩 *module.py* 

```python
import time

def connect() -> None:
    print('Connecting to internet...')
    time.sleep(3)
    print('You are connected!')

# Testing the connect() function
connect()
```

Running `python module.py`:

```bash
Connecting to internet...
You are connected!
```

#### 🧩 *main.py*

```python
from module import connect

connect()
```

Running `python main.py`:

```bash
Connecting to internet...
You are connected!
Connecting to internet...
You are connected!
```

Running `python main.py` prints it twice, because `connect()` runs:

1. once during `import module` (module executed on import)
2. once from `connect()` in `main.py`

> Every time you import a module it has to execute the module top-to-bottom, which means it's reading every line and since at the bottom of `module.py` we actually run the function it reads that line as well so it runs the function.

First good habit is to always check that `if __name__ == '__main__'` and put `connect()` inside that. This will ensure that code will only be run if we run the module directly (`python module.py`).

#### 🧩 *module.py*

```python
import time

def connect() -> None:
    print('Connecting to internet...')
    time.sleep(3)
    print('You are connected!')

# Testing the connect() function
if __name__ == '__main__':
    # Runs only when executed directly: python module.py
    connect()
```

Running again `python main.py`:

```bash
Connecting to internet...
You are connected!
```

Everything works as expected, this time `connect()` runs only once.

---
### 2. `main()`

```python
def greet() -> None:
    print('Hello, world!')

def bye() -> None:
    print('Bye, world!')

def main() -> None:
    greet()
    bye()

if __name__ == '__main__':
    main()
```

A good habit is to define a main entry point. It's a clean way to organize your code and it's more readable.

---
### 3. Big functions

```python
def enter_club(name: str, age: int, has_id: bool) -> None:
    if name.lower() == 'bob':
        print('Get out of here Bob, we don\'t want no trouble')
        return

    if age >= 21 and has_id:
        print('You may enter the club.')
    else:
        print('You may not enter the club.')

def main() -> None:
    enter_club('Bob', 29, has_id=True)
    enter_club('James', 29, has_id=True)
    enter_club('Sandra', 29, has_id=False)
    enter_club('Mario', 20, has_id=True)

if __name__ == '__main__':
    main()
```

The function works nicely, but is not so good in terms of reusability. It's a bad practice because it's handling far too much functionality in a single function.

```python
def is_an_adult(age: int, has_id: bool) -> bool:
    return age >= 21 and has_id

def is_bob(name: str) -> bool:
    return name.lower() == 'bob'

def enter_club(name: str, age: int, has_id: bool) -> None:
    if is_bob(name):
        print('Get out of here Bob, we don\'t want no trouble')
        return

    if is_an_adult(age, has_id):
        print('You may enter the club.')
    else:
        print('You may not enter the club.')

def main() -> None:
    enter_club('Bob', 29, has_id=True)
    enter_club('James', 29, has_id=True)
    enter_club('Sandra', 29, has_id=False)
    enter_club('Mario', 20, has_id=True)

if __name__ == '__main__':
    main()
```

Breaking a big function into smaller parts increases reuse and testability. It's a good habit to keep your functions as simple and reusable as possible.

---
### 4. Type Annotations

Without type annotations, it’s unclear what this function expects:

```python
def upper_everything(elements):
    return [element.upper() for element in elements]
```

If someone passes integers, it will fail at runtime because integers don’t have an `.upper()` method. You also won’t get any early warnings from your editor or tooling.

Be explicit from the beginning:

```python
def upper_everything(elements: list[str]) -> list[str]:
    return [element.upper() for element in elements]
```

Type annotations make the function self-documenting: the signature tells you what goes in and what comes out.

They also help IDEs and type checkers (e.g., `mypy`, `pyright`) catch mistakes earlier.

```python
def upper_everything(elements: list[str]) -> list[str]:
    return [element.upper() for element in elements]

my_list: list[int] = upper_everything(['a', 'b', 'c'])
```

```
mypy main.py
main.py:7: error: Incompatible types in assignment (expression has type "list[str]", variable has type "list[int]")  [assignment]
Found 1 error in 1 file (checked 1 source file)
```

---
### 5. List comprehensions

```python
people: list[str] = ['James', 'Charlotte', 'Stephany', 'Mario', 'Sandra']

long_names: list[str] = []
for person in people:
    if len(person) > 7:
        long_names.append(person)

print(f'Long names: {long_names}')
```

List comprehensions are often faster and can be easier to read.

```python
long_names: list[str] = [person for person in people if len(person) > 7]
print(f'Long names: {long_names}')
```
