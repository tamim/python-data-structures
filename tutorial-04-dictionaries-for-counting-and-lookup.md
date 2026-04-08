---
title: Tutorial 4: Dictionaries for Counting and Lookup
---

# Tutorial 4: Dictionaries for Counting and Lookup

## 1. What this tutorial is about

So far in this series, the main message has been:

**choose the data structure that matches the real job**

This tutorial is about one of the most useful tools in competitive programming:

**the dictionary**

If the real question is:

- how many times did each value appear?
- what value belongs to this key?
- where did this item appear?

then a dictionary is often the right tool.

A dictionary is not a harder list.

It is for linking one thing to another:

- value to count
- label to value
- item to position

## 2. A familiar student instinct or list-based approach

Students often avoid dictionaries because lists feel more familiar.

So they try to force counting into list-shaped code:

```python
nums = [4, 7, 4, 2, 7, 7]
different = []
counts = []

for x in nums:
    if x in different:
        index = different.index(x)
        counts[index] += 1
    else:
        different.append(x)
        counts.append(1)

print(different)
print(counts)
```

This works, but it is awkward.

The code is trying to keep two lists lined up:

- one for the values
- one for the counts

That is a clue that the real job is not “use two matching lists”.

Students often do the same thing for lookup problems too:

```python
names = ["Ava", "Mia", "Noah"]
scores = [18, 15, 21]

target = "Mia"
index = names.index(target)

print(scores[index])
```

Again, this can work. But the code is forcing a lookup problem into a pair of lists.

## 3. What goes wrong

The first problem is that the code becomes harder to think about.

If one list gets out of sync with the other, the whole structure becomes confusing.

The second problem is that repeated work is hiding inside the list approach.

In this line:

```python
if x in different:
```

Python checks through the list.

Then in this line:

```python
index = different.index(x)
```

Python checks through the list again.

So the code is not only awkward. It is doing extra searching too.

There is a clarity problem as well. If the real task is:

- value -> count
- name -> score
- item -> first position

then a dictionary matches that idea directly.

A list does not.

## 4. A better way to think about the problem

Before writing code, ask:

**What is the real relationship in this problem?**

If the problem is about:

- each value and its count
- each label and its matching value
- each item and the position where it appeared

then you are dealing with a **key -> value** relationship.

That is dictionary thinking.

A dictionary gives you:

- a key
- the value linked to that key

Examples:

- `7 -> 3` means the number `7` appeared three times
- `"Mia" -> 15` means Mia's score is 15
- `"cat" -> 4` means `"cat"` first appeared at index 4

Once you see the problem that way, dictionary code usually becomes much easier to design.

A dictionary is not for indexing by position, and it is not a replacement for a list in every problem. It is simply the right structure when the problem sounds like “this key links to that value”.

## 5. The better tool or pattern

Here is the counting problem again, this time with a dictionary:

```python
nums = [4, 7, 4, 2, 7, 7]
freq = {}

for x in nums:
    freq[x] = freq.get(x, 0) + 1

print(freq)
```

This pattern means: get the current count for `x`, use `0` if `x` is not present yet, then store the updated count.

That is one of the most useful dictionary patterns in competitive programming.

Here is a simple lookup example:

```python
scores = {
    "Ava": 18,
    "Mia": 15,
    "Noah": 21
}

print(scores["Mia"])
```

Another good use is position tracking.

```python
nums = [8, 3, 8, 5, 3, 9]
first_pos = {}

for i in range(len(nums)):
    x = nums[i]
    if x not in first_pos:
        first_pos[x] = i

print(first_pos)
```

That gives each value a linked position.

The same idea works for many contest tasks:

- first position
- last position
- best score so far
- label -> answer lookup

Dictionary problems often become clearer as soon as you name the key and the value out loud.

If you can say the relationship clearly in words, you can usually design the dictionary clearly in code.

You can also track the last position instead:

```python
nums = [8, 3, 8, 5, 3, 9]
last_pos = {}

for i in range(len(nums)):
    last_pos[nums[i]] = i

print(last_pos)
```

## 6. Common patterns

### Pattern A: Frequency counting

```python
freq = {}

for x in nums:
    freq[x] = freq.get(x, 0) + 1
```

### Pattern B: Key -> value lookup

```python
capital = {
    "NSW": "Sydney",
    "VIC": "Melbourne",
    "QLD": "Brisbane"
}

print(capital["NSW"])
```

### Pattern C: First position of each value

```python
first_pos = {}

for i in range(len(nums)):
    if nums[i] not in first_pos:
        first_pos[nums[i]] = i
```

### Pattern D: Linking labels to information

```python
phone_book = {
    "Ava": "0400",
    "Mia": "0411"
}

print(phone_book["Ava"])
```

If the problem sounds like “this thing belongs to that label”, dictionary thinking fits well.

## 7. Common mistakes

### Mistake 1: Using a dictionary when you really need indexing

You cannot treat a dictionary like a list.

If a problem asks for the value at index `3`, or asks you to compare neighbours in a sequence, a list is probably the better tool.

### Mistake 2: Using `d[key]` when the key might not exist

Use `d[key]` when the key should definitely exist.

Use `.get()` when the key may be missing and you want a default value:

```python
scores = {"Ava": 18, "Mia": 15}
print(scores.get("Noah", 0))
```

In that example, `0` is the fallback value.

So the rule is:

- use `d[key]` when the key should definitely exist
- use `.get()` when the key may be missing and you want a default value

### Mistake 3: Trying to use a list as a dictionary key

This fails:

```python
positions = {}
positions[[2, 5]] = "blocked"
```

Because dictionary keys must be **hashable**.

Lists are mutable, so they are not hashable. Tuples are often the right replacement:

```python
positions = {}
positions[(2, 5)] = "blocked"

print(positions[(2, 5)])
```

This matters when you need a compound key such as a row and column, two values together, or a pair of labels.

### Mistake 4: Avoiding dictionaries because they look harder

Dictionaries often make the code simpler because they match the real job directly.

If you find yourself building parallel lists for counts, scores, or positions, stop and ask whether a dictionary would say the idea more clearly.

## 8. Short self-check practice

Try these before moving on.

1. A problem asks how many times each card value appears. Why should a dictionary come to mind?
2. What does `freq[x] = freq.get(x, 0) + 1` do?
3. Why does `positions[[3, 4]] = 1` fail, and what could you use instead?

Suggested answers:

1. Because the real job is value -> count.
2. It updates the count for `x`, using `0` if `x` is not already in the dictionary.
3. It fails because lists are not hashable. A tuple like `(3, 4)` is often the right replacement.
