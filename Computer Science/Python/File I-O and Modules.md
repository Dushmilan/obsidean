# File I/O & Modules

Real programs read data, write results, and split code across files. File handling in Python is built around the `with` context manager — which guarantees cleanup even on exceptions. Modules and packages organize code into importable units, and the standard library is a treasure chest you should never reinvent.

**The Intuition:** A file is a stream of bytes/characters with a cursor. `open()` gives you a file object; `with` ensures it's closed when the block ends — no leaks, even on errors. A module is just a `.py` file; importing it runs it once and makes its names available. Packages are folders of modules with an `__init__.py`.

## File I/O — the `with` statement

```python
# Read — the three standard ways
with open("data.txt", "r") as f:
    content = f.read()            # whole file → one string
    lines = f.readlines()         # → list of lines (with \n)

with open("data.txt") as f:
    for line in f:                # line-by-line — O(1) memory, best for big files
        print(line.strip())

# Write
with open("out.txt", "w") as f:   # "w" OVERWRITES — danger!
    f.write("Hello\n")
    f.write("World\n")

# Append
with open("log.txt", "a") as f:
    f.write(f"{message}\n")

# Read + write
with open("data.txt", "r+") as f:     # r+ = read/write at cursor
    ...

# Binary
with open("img.bin", "rb") as f:      # "b" suffix — bytes not str
    raw = f.read()
```

**File modes:** `"r"` read (default), `"w"` write (truncates!), `"a"` append, `"x"` exclusive create (fails if exists), `"r+"` read/write, plus `"b"` for binary.

## Common structured formats

```python
import json, csv

# JSON
data = {"name": "Ada", "scores": [92, 85]}
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)          # serialize → file
with open("data.json") as f:
    restored = json.load(f)               # file → object
# json.dumps / json.loads — to/from STRINGS (for APIs, network)

# CSV
with open("grades.csv", "r", newline="") as f:
    for row in csv.DictReader(f):         # dict per row, keyed by header
        print(row["name"], row["score"])

with open("out.csv", "w", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "score"])
    writer.writeheader()
    writer.writerow({"name": "Ada", "score": 92})
```

## Paths — use pathlib

```python
from pathlib import Path

p = Path("data/sub/file.txt")
p.exists()            # True/False
p.is_file() / p.is_dir()
p.name                # "file.txt"
p.stem                # "file"
p.suffix              # ".txt"
p.parent              # data/sub
p.read_text()         # whole file as str — one-liner
p.write_text("hello") # write whole string
p.glob("*.md")        # files matching pattern in dir
Path.home()           # user home
Path.cwd()            # current working dir
```

## Modules — import machinery

```python
import math                    # whole module
math.sqrt(16)                  # 4.0

from math import sqrt, pi      # specific names
sqrt(16)

from math import *             # EVERYTHING — avoid: pollutes namespace, hides origin

import numpy as np             # aliasing — common for long names
np.array([1, 2, 3])

# Importing your own file: same folder → just works
# utils.py  →  import utils ; utils.helper()
# from utils import helper
```

**Module facts:**
- Importing runs the module **once**, then caches it in `sys.modules` — second import is instant
- `if __name__ == "__main__":` — runs only when executed directly, not when imported:
  ```python
  def main():
      print("doing work")
  if __name__ == "__main__":
      main()
  ```
- `__all__ = ["name1", "name2"]` in a module controls what `from mod import *` exposes

## Packages

```
mypkg/
├── __init__.py        # marks it a package (may be empty)
├── core.py
└── sub/
    ├── __init__.py
    └── helpers.py

# import mypkg.core          → mypkg.core.func()
# from mypkg.sub.helpers import helper
# from mypkg import core
```

## Standard library highlights (never reinvent)

```python
import os            # os.path.join, os.listdir, os.environ
import sys           # sys.argv (CLI args), sys.exit, sys.path
import re            # regex — re.search, re.findall, re.sub
import random        # random.randint, random.choice, random.shuffle
import math          # math.sqrt, math.gcd, math.factorial, math.comb
import datetime      # datetime.date.today(), timedelta
import collections   # Counter, defaultdict, deque, namedtuple
import itertools     # product, combinations, permutations, chain
import functools     # reduce, lru_cache, partial
import statistics    # mean, median, stdev
import subprocess    # run shell commands from Python
import urllib.request  # fetch URLs (or use requests, a 3rd-party lib)
```

---

**Setup:** Read a CSV of `name,score`, compute the average score, and append a row.

**Solution:**
```python
import csv

rows = []
with open("grades.csv", newline="") as f:
    for row in csv.DictReader(f):
        rows.append(row)

avg = sum(int(r["score"]) for r in rows) / len(rows)
print(f"Average: {avg:.1f}")

with open("grades.csv", "a", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "score"])
    writer.writerow({"name": "Summary", "score": f"{avg:.1f}"})
```

**Key insight:** `csv.DictReader` maps headers to values — you never parse commas yourself. Reading in one block, then appending with mode `"a"`, keeps the file intact.

---

**Setup:** Merge two JSON config files, with the second overriding the first.

**Solution:**
```python
import json
from pathlib import Path

def load(p):
    return json.loads(Path(p).read_text())

merged = {**load("defaults.json"), **load("override.json")}
Path("merged.json").write_text(json.dumps(merged, indent=2))
```

**Key insight:** `Path.read_text` + `json.loads` reads a JSON file in one line; dict unpacking merges with "later wins" override semantics.

---

**Setup:** Find all `.md` files in a directory tree and count the total lines.

**Solution:**
```python
from pathlib import Path

total = 0
for md in Path("notes").rglob("*.md"):     # recursive glob
    total += sum(1 for _ in md.open())
print(f"{total} lines across {md} files")
```

**Key insight:** `rglob` walks recursively. `sum(1 for _ in f)` counts lines without loading the file — memory stays flat even for huge files.

---

**Setup:** Make a script runnable both as `python script.py` and importable.

**Solution:**
```python
def main():
    print("Hello from the script!")

if __name__ == "__main__":
    main()
```

**Key insight:** When run directly, `__name__` is `"__main__"`; when imported, it's the module name — so `main()` runs only on direct execution. Every reusable script should have this guard.

---

## Practice (try before peeking)

1. Why is `with open(...)` better than `f = open(...)` without closing?
2. `open("out.txt", "w")` on a file you care about — what just happened?
3. What's `sys.argv` for a run of `python my.py a b`?

<details><summary>Answers</summary>

1. `with` guarantees `close()` even when an exception propagates — no leaked file handles, no unflushed writes. Forgetting to close can leave data un-written to disk.
2. The file was **truncated to zero bytes** — `"w"` destroys existing content. Use `"a"` (append) or `"x"` (exclusive create) to be safe.
3. `["my.py", "a", "b"]` — `sys.argv[0]` is the script name, the rest are arguments.

</details>

---

**Common traps:**
- `"w"` overwrites silently — never open a precious file in write mode casually
- Text vs binary: `"rb"`/`"wb"` for images/bytes; text mode does newline translation (`\r\n` ↔ `\n`) on Windows
- `read()` vs `readlines()` vs iteration: whole file vs list vs lazy — choose by memory constraints
- Import cycles (A imports B, B imports A) — restructure so dependencies flow one way
- `from x import *` pollutes scope — prefer explicit imports

---
