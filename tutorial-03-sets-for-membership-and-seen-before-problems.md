---
title: Tutorial 3: Sets for Membership and Seen-Before Problems
---

## 1. What this tutorial is about

In the last tutorial, the main lesson was that list-based code can look fine and still lead to slow contest solutions.

This tutorial is about the tool that often fixes that mistake.

If the real question is:

- have I seen this before?
- is this already present?
- how many different values are there?

then a **set** is often the right tool.

A set is not just a different kind of list.

It is built for a different job:

- membership
- uniqueness
- tracking seen values
- counting distinct values

That is the instinct this tutorial is trying to build.

## 2. A familiar student instinct or list-based approach

A common student habit is to use a list to remember what has already appeared.

```python
nums = [4, 7, 2, 9, 7, 5]
seen = []
has_duplicate = False

for x in nums:
    if x in seen:
        has_duplicate = True
        break
    seen.append(x)

print(has_duplicate)
```

This works, but the tool does not match the job very well.

Here is another example:

```python
scores = [12, 18, 12, 9, 18, 15]
different = []

for score in scores:
    if score not in different:
        different.append(score)

print(len(different))
```

Again, the code works, but it is still list-shaped thinking for a uniqueness problem.

The pattern is familiar, but the tool does not match the job very well.

## 3. What goes wrong

The main issue is that a list is not designed for membership and uniqueness.

When you write:

```python
if x in seen:
```

and `seen` is a list, Python checks through the list item by item.

A set works differently. In informal terms, Python can use the value to work out where to look instead of scanning from the first element to the last one.

That is why `x in my_set` usually behaves much more like a direct membership check.

This matters in competitive programming because sample tests are small, but real contest inputs can be much larger.

There is another problem too.

If the real job is “keep only distinct values” or “check whether this is already present”, then a list makes you do extra work to force that behaviour. That usually means the structure choice is off.

## 4. A better way to think about the problem

Before writing code, ask:

**What is the real job here?**

If the real job is one of these:

- membership
- uniqueness
- seen-before tracking
- counting distinct values

then a set should come to mind quickly.

A set stores values, keeps only one copy of each value, and is natural for asking “is this here?”

It also does **not** give you some list features:

- no indexing
- no keeping duplicates
- no reliable position-based order to use in problems

If order matters, a set is usually the wrong choice. If duplicates matter, a set is usually the wrong choice.

If you need consistent sorted output, you would usually convert the set first, for example with `sorted(my_set)`.

Using a set does not mean giving up ordered output forever.

## 5. The better tool or pattern

Here is the duplicate-detection example again, this time with a set:

```python
nums = [4, 7, 2, 9, 7, 5]
seen = set()
has_duplicate = False

for x in nums:
    if x in seen:
        has_duplicate = True
        break
    seen.add(x)

print(has_duplicate)
```

This is a better fit because the logic matches the job.

The variable name `seen` says exactly what the structure is doing, and that pattern appears constantly in contest problems:

- duplicate detection
- visited tracking
- checking whether a value is already present

For counting distinct values, the idea can be even shorter:

```python
scores = [12, 18, 12, 9, 18, 15]
different = set(scores)

print(len(different))
```

Another useful pattern is visited tracking:

```python
visited = set()

positions = [(0, 0), (0, 1), (1, 1), (0, 1)]

for pos in positions:
    if pos in visited:
        print("visited before:", pos)
    else:
        visited.add(pos)
```

You can also combine a set with sorting when you want unique values in a clean order:

```python
nums = [8, 3, 8, 5, 3, 9]
unique_sorted = sorted(set(nums))

print(unique_sorted)
```

That is a very common contest pattern:

- the set removes duplicates
- `sorted(...)` puts the remaining values into a list in increasing order

## 6. Common patterns

Here are some set-friendly patterns that appear often in contest problems.

### Pattern A: Duplicate detection

```python
seen = set()

for x in nums:
    if x in seen:
        print("duplicate found")
        break
    seen.add(x)
```

### Pattern B: Counting distinct values

```python
nums = [5, 5, 2, 8, 2, 2, 9]
different_values = set(nums)

print(len(different_values))
```

### Pattern C: Shared values between groups

```python
team_a = {"Ava", "Mia", "Zoe"}
team_b = {"Mia", "Luca", "Zoe"}

common = team_a & team_b
print(common)
```

Sets are strong when the question is about presence, distinct values, or overlap between groups.

Another very common membership pattern is tracking what is present right now:

```python
present = {"Ava", "Mia", "Noah"}

print("Mia" in present)
print("Luca" in present)
```

That is set thinking in plain form. The job is not sequence storage. The job is presence.

You do not need to memorise every set operation right now. Just notice the pattern: sets are strong when the question is about shared values or presence.

Once that instinct becomes automatic, a lot of list-heavy code starts to look suspicious much earlier.

## 7. Common mistakes

### Mistake 1: Using `{}` for an empty set

```python
empty = {}
print(type(empty))
```

That creates a dictionary, not a set.

To make an empty set, use:

```python
empty = set()
```

### Mistake 2: Expecting indexing

```python
values = {10, 20, 30}
print(values[0])
```

A set is not for indexing. If a problem needs first, last, next, or position `i`, be suspicious of using a set.

### Mistake 3: Forgetting that duplicates disappear

```python
nums = [4, 4, 4, 2]
print(set(nums))
```

That is useful for uniqueness problems, but wrong when repeated values matter.

This is why you should still ask whether duplicates are important before you reach for a set.

### Mistake 4: Confusing `remove()` and `discard()`

`remove()` raises an error if the value is missing. `discard()` does not.

```python
names = {"Ava", "Mia", "Noah"}
names.discard("Luca")
```

That is useful when the value may or may not be there.

If you know the value must be present, `remove()` is fine. If you are not sure, `discard()` is safer.

### Mistake 5: Trying to add a list to a set

This fails:

```python
seen = set()
seen.add([1, 2])
```

Because set elements must be **hashable**.

In simple terms, that means the value must be a type Python can safely use as a stable set element.

Lists are mutable, so they are not hashable. Because a list can be changed later with operations like `.append()`, Python cannot trust it to stay in the same place inside a set.

Tuples are often the right replacement:

```python
seen = set()
seen.add((1, 2))

print((1, 2) in seen)
```

This matters a lot in grid and coordinate problems. If you want to store a position, `(r, c)` is usually the right shape, not `[r, c]`.

The same hashability idea will matter again in the next tutorial when dictionary keys come up.

## 8. Short self-check practice

Try these before moving on.

1. A problem asks whether any student ID appears more than once. Why should a set come to mind?
2. Why is a set a poor choice if the problem needs the 3rd element or the element at index `i`?
3. Why does `seen.add([2, 5])` fail, and what could you use instead?

Suggested answers:

1. Because the real task is seen-before checking and membership.
2. Because sets do not support indexing.
3. It fails because lists are not hashable. A tuple like `(2, 5)` is often the right replacement.

The next big question after membership is counting and lookup. That is where dictionary thinking becomes the better fit.
