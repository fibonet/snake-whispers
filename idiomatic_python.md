### Looping over a range of numbers

**Worse**
```python
for i in [0, 1, 2, 3, 4, 5]:
    print(i**2)
```

**Better** → Using `range`
```python
for i in range(6):
    print(i**2)
```

---
### Looping over a collection

**Worse**
```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors)):
    print(colors[i])
```

**Better**
```python
colors = ['red', 'green', 'blue', 'yellow']

for color in colors:
    print(color)
```

---
### Looping backwards

**Worse**
```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors) -1, -1, -1):
    print(colors[i])
```

**Better**  → using `reversed`
```python
colors = ['red', 'green', 'blue', 'yellow']

for color in reversed(colors):
    print(color)
```

---
### Looping over a collection and indices

**Worse**
```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors)):
    print(i, '--->', colors[i])
```

**Better** → using `enumerate`
```python
colors = ['red', 'green', 'blue', 'yellow']

for i, color in enumerate(colors):
    print(i, '--->', color)
```

---
### Looping over two collections

**Worse**
```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue', 'yellow']

n = min(len(names), len(colors))
for i in range(n):
    print(names[i], '--->', colors[i])
```

**Better** → using `zip`
```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue', 'yellow']

for name, color in zip(names, colors):
    print(name, '--->', color)
```

---
### Looping in sorted order

```python
colors = ['red', 'green', 'blue', 'yellow']

for color in sorted(colors):
    print(color)

for color in sorted(colors, reverse=True):
    print(color)
```

---
### Custom sort order

**Worse**
```python
from functools import cmp_to_key

colors = ['red', 'green', 'blue', 'yellow']

def compare_length(c1, c2):
    if len(c1) < len(c2):
        return -1
    if len(c1) > len(c2):
        return 1
    return 0

print(sorted(colors, key=cmp_to_key(compare_length)))
```

**Better**
```python
print(sorted(colors, key=len))
```

---
### Call a function until a sentinel value

**Worse**
```python
blocks = []
while True:
    block = f.read(32)
    if block == '':
        break
    blocks.append(block)
```

**Better**  → Using `partial`
```python
from functools import partial

blocks = []
for block in iter(partial(f.read, 32), ''):
    blocks.append(block)
```

---
### Distinguishing multiple exit points in loops

**Worse**
```python
def find(seq, target):
    found = False
    for i, value in enumerate(seq):
        if value == target:
            found = True
            break
    if not found:
        return -1
    return i
```

**Better**
```python
def find(seq, target):
    for i, value in enumerate(seq):
        if value == target:
            break
    else:
        return -1
    return i
```

---
### Looping over dictionary keys

**Worse**
```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

for k in list(d.keys()):
    if k.startswith('r'):
        del d[k]

for k in d:
    print(k)
```

**Better**
```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

d = {k : d[k] for k in d if not k.startswith('r')}

for k in d:
    print(k)
```

---
### Looping over dictionary keys and values

**Worse**
```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

for k in d:
    print(k, '--->', d[k])
```

**Better**
```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

for k, v in d.items():
    print(k, '--->', v)
```

---
### Construct a dictionary from pairs

```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue']

d = dict(zip(names, colors))
```

---
### Counting with dictionaries

**Worse**
```python
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = {}
for color in colors:
    if color not in d:
        d[color] = 0
    d[color] += 1
```

**Better**
```python
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = {}
for color in colors:
    d[color] = d.get(color, 0) + 1
```

**Best**
```python
from collections import defaultdict

colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = defaultdict(int)
for color in colors:
    d[color] += 1
```

---
### Grouping with dictionaries

**Worse**
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

d = {}
for name in names:
    key = len(name)
    if key not in d:
        d[key] = []
    d[key].append(name)
```

**Better**
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

d = {}
for name in names:
    key = len(name)
    d.setdefault(key, []).append(name)
```

**Best**
```python
from collections import defaultdict

names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

d = defaultdict(list)
for name in names:
    key = len(name)
    d[key].append(name)
```

---
### Is a dictionary popitem() atomic?

```python
# Yes, it is atomic. It can be used between threads.

d = {'matthew': 'blue', 'rachel':'green', 'raymond':'red'}

while d:
    key, value = d.popitem()
    print(key, '-->', value)
```

---
### Linking dictionaries

**Worse**
```python
import argparse
import os

defaults = {'color': 'red', 'user': 'guest'}

parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args([])
command_line_args = {k:v for k, v in vars(namespace).items() if v}

d = defaults.copy()
d.update(os.environ)
d.update(command_line_args)
```

**Better**  → using `ChainMap`
```python
import argparse
import os
from collections import ChainMap

defaults = {'color': 'red', 'user': 'guest'}

parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args([])
command_line_args = {k:v for k, v in vars(namespace).items() if v}

d = ChainMap(command_line_args, os.environ, defaults)
```

---
### Clarify function calls with keyword arguments

```python
twitter_search('@obama', False, 20, True)

twitter_search('@obama', retweets = False, numtweets = 20, popular = True)
```

---
### Clarify multiple return values with named tuples

```python
from collections import namedtuple

doctest.testmod()
# older doctest returns (0,4)

doctest.testmod()
# newer doctest returns TestResults(failed=0, attempted=4)
# which is a better output

# the way you make a namedtuple:
TestResults = namedtuple('TestResults', ['failed','attempted'])
```

---
### Unpacking sequences

**Worse**
```python
p = 'Raymond', 'Hettinger', 0x30, 'python@example.com'

fname = p[0]
lname = p[1]
age = p[2]
email = p[3]
```

**Better**
```python
p = 'Raymond', 'Hettinger', 0x30, 'python@example.com'

fname, lname, age, email = p
```

---
### Updating multiple state variables

**Worse**
```python
def fibonacci(n):
    x = 0
    y = 1
    for i in range(n):
        print(x)
        t = y
        y = x + y
        x = t
```

**Better**
```python
def fibonacci(n):
    x, y = 0, 1
    for i in range(n):
        print(x)
        x, y = y, x + y
```

---
### Concatenating strings

**Worse**
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

s = names[0]
for name in names[1:]:
    s += ', ' + name

print(s)
```

**Better**
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

print(', '.join(names))
```

---
### Updating sequences

**Worse**
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

del names[0]
names.pop(0)
names.insert(0, 'mark')
```

**Better**
```python
from collections import deque

# use the correct data structure
names = deque(['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie'])

del names[0]
names.popleft()
names.appendleft('mark')
```

---
### Using decorators to factor-out administrative logic

**Worse**
```python
def web_lookup(url, saved = {}):
    if url in saved:
        return saved[url]
    page = urllib.urlopen(url).read()
    saved[url] = page
    return page
```

**Better**
```python
from functools import cache

@cache
def web_lookup(url):
    return urllib.request.urlopen(url).read()
```

---
### How to open and close files

**Worse**
```python
f = open('data.txt')
try:
    data = f.read()
finally:
    f.close()
```

**Better**
```python
with open('data.txt') as f:
    data = f.read()
```

---
### How to use locks

**Worse**
```python
# Make a lock
lock = threading.Lock()

# Old-way to use a lock
lock.acquire()
try:
    print('Critical section 1')
    print('Critical section 2')
finally:
    lock.release()
```

**Better**
```python
# Make a lock
lock = threading.Lock()

# New-way to use a lock
with lock:
    print('Critical section 1')
    print('Critical section 2')
```

---
### Factor-out temporary contexts (suppress)

**Worse**
```python
import os

try:
    os.remove('someFile.tmp')
except OSError:
    pass
```

**Better**
```python
import os
from contextlib import suppress

with suppress(OSError):
    os.remove('someFile.tmp')
```

---
### Factor-out temporary contexts (redirect_stdout)

**Worse**
```python
with open('help.txt', 'w') as f:
    oldstdout = sys.stdout
    sys.stdout = f
    try:
        help(pow)
    finally:
        sys.stdout = oldstdout
```

**Better**
```python
from contextlib import redirect_stdout

with open('help.txt', 'w') as f:
    with redirect_stdout(f):
        help(pow)
```

---
### List Comprehensions and Generator Expressions

**Worse**
```python
result = []
for i in range(10):
    s = i**2
    result.append(s)
print(sum(result))
```

**Better**
```python
print(sum(i**2 for i in range(10)))
```

