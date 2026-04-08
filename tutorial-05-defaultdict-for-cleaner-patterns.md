---
title: Tutorial 5: defaultdict for Cleaner Patterns
---

## 1. What this tutorial is about

In the last tutorial, the main lesson was that dictionaries are a natural tool for counting, lookup, and position tracking.

This tutorial is the next step.

It is **not** about a magical new data structure.

It is about a cleaner way to write some dictionary patterns you already know.

That tool is `defaultdict`, from Python's `collections` module.

The main idea is simple:

**`defaultdict` does not replace dictionary thinking. It makes familiar dictionary code shorter and cleaner.**

## 2. A familiar student instinct or standard dictionary-based approach

Start with a pattern you already know: frequency counting.

With a normal dictionary, you might write:

```python
nums = [4, 7, 4, 2, 7, 7]
freq = {}

for x in nums:
    freq[x] = freq.get(x, 0) + 1

print(freq)
```

For grouping, you might write:

```python
students = [
    ("Ava", "Blue"),
    ("Mia", "Red"),
    ("Noah", "Blue"),
    ("Zoe", "Red")
]

groups = {}

for name, house in students:
    if house not in groups:
        groups[house] = []
    groups[house].append(name)

print(groups)
```

There is nothing wrong with either version.

You should still understand the normal dictionary version first. `defaultdict` only helps because the underlying pattern is already clear.

## 3. What goes wrong or becomes repetitive

The problem is not correctness.

The problem is that some dictionary patterns repeat the same setup again and again.

In grouping code like this:

```python
if house not in groups:
    groups[house] = []
groups[house].append(name)
```

you are repeatedly saying:

- check whether the key exists
- if not, create the default value

That is exactly the repetition `defaultdict` is designed to remove.

So the question becomes:

**Can Python help express that “missing key starts with a default value” idea more directly?**

## 4. A better way to think about the problem

Do not think of `defaultdict` as a mystery object.

Think of it like this:

- you still have keys
- you still have values
- you tell Python what value to create when a missing key is used

For example:

- `defaultdict(int)` gives missing keys a starting value like `0`
- `defaultdict(list)` gives missing keys a starting value like `[]`
- `defaultdict(set)` gives missing keys a starting value like `set()`

So the idea is not new. You were already doing this yourself with normal dictionary code.

You were already thinking:

- missing count starts at `0`
- missing group starts as an empty list
- missing group starts as an empty set

`defaultdict` just lets you express that more cleanly.

Present it as a cleaner expression of a pattern you already understand, not as a shortcut to memorise blindly.

If students meet `defaultdict` too early, it can feel magical. After normal dictionary patterns are clear, it feels natural.

## 5. The better tool or pattern

First, import it:

```python
from collections import defaultdict
```

### Counting with `defaultdict(int)`

You already know the normal pattern:

```python
freq[x] = freq.get(x, 0) + 1
```

`defaultdict` lets you write the same idea more directly:

```python
from collections import defaultdict

nums = [4, 7, 4, 2, 7, 7]
freq = defaultdict(int)

for x in nums:
    freq[x] += 1

print(freq)
```

Because `int()` gives `0`, a missing key starts at `0`, then `+= 1` works naturally.

The underlying idea is still the same as:

```python
freq[x] = freq.get(x, 0) + 1
```

So `defaultdict(int)` should feel familiar, not magical.

### Grouping with `defaultdict(list)`

```python
from collections import defaultdict

students = [
    ("Ava", "Blue"),
    ("Mia", "Red"),
    ("Noah", "Blue"),
    ("Zoe", "Red")
]

groups = defaultdict(list)

for name, house in students:
    groups[house].append(name)

print(groups)
```

This is the same grouping idea as before, written without the repeated missing-key setup.

### Collecting multiple positions

```python
from collections import defaultdict

nums = [8, 3, 8, 5, 3, 8]
positions = defaultdict(list)

for i in range(len(nums)):
    positions[nums[i]].append(i)

print(positions)
```

Now each number is linked to a list of positions.

This kind of pattern appears often in contest problems, especially when one value may appear many times and you want to remember every place it showed up.

The job is still dictionary-shaped. The code is just cleaner.

## 6. Common patterns

### Pattern A: Cleaner frequency counting

```python
from collections import defaultdict

freq = defaultdict(int)

for x in nums:
    freq[x] += 1
```

### Pattern B: Grouping values by key

```python
from collections import defaultdict

groups = defaultdict(list)

for name, house in students:
    groups[house].append(name)
```

### Pattern C: Grouping without duplicates with `defaultdict(set)`

```python
from collections import defaultdict

clubs = [
    ("Ava", "chess"),
    ("Ava", "maths"),
    ("Mia", "chess"),
    ("Ava", "chess")
]

joined = defaultdict(set)

for name, club in clubs:
    joined[name].add(club)

print(joined)
```

Use `defaultdict` when the problem is already dictionary-shaped and you keep repeating “if missing, create a default value”.

Another common case is collecting multiple positions for each number:

```python
from collections import defaultdict

positions = defaultdict(list)

for i in range(len(nums)):
    positions[nums[i]].append(i)
```

That is cleaner than manually checking whether the list of positions exists first.

## 7. Common mistakes

### Mistake 1: Using `defaultdict` before understanding normal `dict`

It should not feel magical.

`defaultdict(int)` is just a cleaner expression of `freq[x] = freq.get(x, 0) + 1`.

### Mistake 2: Thinking `defaultdict` is always necessary

Sometimes a normal dictionary is completely fine, especially for simple direct lookup.

Use `defaultdict` when it removes repeated setup or checking code, not just because it exists.

For example, direct lookup like `scores["Ava"]` does not usually need `defaultdict` at all.

### Mistake 3: Forgetting that missing keys can be created automatically

This behaviour is useful, but it can also cause a subtle bug.

```python
from collections import defaultdict

freq = defaultdict(int)
print("score" in freq)

value = freq["score"]

print("score" in freq)
```

The first `print` is `False`.

After `freq["score"]` is accessed, the key is created automatically, so the second `print` becomes `True`.

If you only want to check whether a key exists, be careful not to create it by accident.

If the printed result says `defaultdict(<class 'list'>, ...)`, do not let that distract you. The useful part is still the key -> value mapping inside it.

## 8. Short self-check practice

Try these before moving on.

1. Why is `defaultdict(int)` a natural fit for frequency counting?
2. What repeated code does `defaultdict(list)` often remove?
3. What should you be careful about when accessing a missing key in a `defaultdict`?

Suggested answers:

1. Because missing keys start at `0`, so `freq[x] += 1` works cleanly.
2. It removes the repeated “if key not in dictionary, create an empty list” step.
3. Accessing a missing key can create it automatically.
