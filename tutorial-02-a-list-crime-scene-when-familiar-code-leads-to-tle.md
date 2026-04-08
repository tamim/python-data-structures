# Tutorial 2: A List Crime Scene: When Familiar Code Leads to TLE

## 1. What this tutorial is about

In Tutorial 1, the goal was to be fair to lists.

This tutorial is about a different problem:

**a list-based solution can look reasonable, work on small inputs, and still be a poor contest solution.**

That is why this tutorial is a crime scene.

The evidence looks harmless at first:

- the code runs
- the answer is correct on tiny tests
- the list-based approach feels familiar

But on larger inputs, the real problem appears:

- the code is slow
- it hides extra work
- it uses a familiar tool for the wrong job

And in a contest, that can lead to **Time Limit Exceeded**, usually shortened to **TLE**.

TLE often means your logic was fine, but your approach was too slow for the real input size.

So the crime scene has a pattern:

- the evidence looks innocent
- the real damage only appears on larger tests

## 2. A familiar student instinct or list-based approach

Let’s start with a very common instinct.

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

This works on small cases, so many students stop here.

Here is another familiar pattern:

```python
line = ["Ava", "Mia", "Luca", "Zoe"]

line.remove("Mia")
line.insert(2, "Noah")

print(line)
```

This also works.

That is the trap. In contest settings, “works” is not enough. You also need to ask whether the code is doing more work than it appears to be doing.

## 3. What goes wrong

The first problem is hidden loops.

Look carefully at this line:

```python
if x in seen:
```

When `seen` is a list, Python checks items one by one until it finds `x` or reaches the end.

So this short line is really doing a scan:

```python
found = False

for value in seen:
    if value == x:
        found = True
        break
```

That hidden scan is easy to miss on small inputs. It is harder to ignore when the list keeps growing and you repeat the same check thousands of times.

If `seen` has 3 items, that is not a big deal. If `seen` has 30,000 items and you keep doing the same check, it becomes expensive.

That is the big-picture cost problem.

One loop inside another loop grows fast.

If `seen` reaches about 50,000 items and you keep checking membership for about 50,000 values, that scanning can add up to billions of comparisons. You do not need formal notation to see the danger: repeated searching inside repeated searching is exactly how a reasonable-looking solution turns into TLE.

Now look at the second pattern:

```python
line.remove("Mia")
line.insert(2, "Noah")
```

List operations like `.remove()` and `.insert()` can force Python to move items around.

If you insert into the middle, later elements shift to the right. If you remove from the middle, later elements shift left. Doing that once is fine. Doing it thousands of times becomes real work.

For example, if this is your line:

```python
line = ["Ava", "Mia", "Luca", "Zoe"]
```

and you do:

```python
line.insert(1, "Ivy")
```

then `"Mia"`, `"Luca"`, and `"Zoe"` all have to move one step to the right.

### A simple speed test you can run yourself

Try this on your computer:

```python
import time

n = 12000
# The duplicate is at the very end to force the worst-case scan.
nums = list(range(n)) + [n - 1]

start = time.perf_counter()
seen = []

for x in nums:
    if x in seen:
        break
    seen.append(x)

list_time = time.perf_counter() - start

start = time.perf_counter()
seen = set()

for x in nums:
    if x in seen:
        break
    seen.add(x)

set_time = time.perf_counter() - start

print("list version:", round(list_time, 4), "seconds")
print("set version:", round(set_time, 4), "seconds")
```

The duplicate is placed at the very end, so the list version gets the least help possible.

The point is simple: both versions are correct, but the list version usually gets slow much sooner.

If `12000` feels slow, try `8000`. If it runs quickly, try `20000`.

That is what TLE can feel like: the same basic idea suddenly stops being good enough when the test data gets bigger.

## 4. A better way to think about the problem

Do not start with:

**What list code can I write?**

Start with:

**What is the real task?**

Ask:

- Do I need order?
- Am I checking whether something has appeared before?
- Am I counting?
- Am I doing lookup?

In the duplicate example, the real task is **membership and seen-before tracking**.

Once you say that clearly, a list should stop feeling like the natural tool.

The same warning applies to repeated `.remove()` and `.insert()` code. Maybe the real task is not “keep editing a list”. Maybe the real task is:

- track who is currently present
- count changes
- store positions by name

If your program keeps doing awkward list surgery, stop and ask whether you chose the wrong structure at the start.

Messy code is often a warning sign that the structure choice is doing the damage.

## 5. The better tool or pattern

For duplicate detection, use a structure designed for membership:

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

The logic is almost the same as the list version, but the thinking is better: the task is membership, so use a structure built for membership.

For repeated `.remove()` and `.insert()` problems, there is not always one magical replacement. But there is still a better habit: name the real job first.

Sometimes the answer is:

- a set for presence
- a dictionary for counts or positions
- a list only for final ordered output

## 6. Red flags in list-heavy code

Be suspicious when you see:

- repeated `if x in seen` on a growing list
- many presence checks against a large list
- repeated `.remove()` and `.insert()` in the middle of a list
- repeated `nums.count(x)` inside a loop

These patterns can still produce correct answers. The problem is that they often hide repeated work and point to the wrong data structure.

For example, this is a warning sign:

```python
nums = [4, 7, 4, 2, 7, 7]

for x in nums:
    print(x, nums.count(x))
```

`nums.count(x)` scans through the list each time, so the program keeps re-counting the same data again and again.

This list is not exhaustive, but it is a useful contest instinct:

- repeated searching inside repeated searching is dangerous
- repeated shifting inside a long list is dangerous
- repeated re-counting of the same list is dangerous

You do not need to panic every time you see one of these. On small data, they may be fine.

The point is to notice when they keep happening inside a large input.

That is the forensic habit this tutorial wants: look past the surface and ask what the code is really doing repeatedly.

## 7. Common mistakes

### Mistake 1: Trusting tiny tests too much

If your code works on 6 numbers, that proves almost nothing about contest speed.

### Mistake 2: Treating `in` as if it is always cheap

`x in my_list` looks short, but on a list it means checking items one by one.

### Mistake 3: Repeating awkward list edits

If your code is full of `.remove()` and `.insert()`, inspect the design instead of just polishing the syntax.

### Mistake 4: Choosing the most familiar structure first

In contests, “I know lists best” is not a strong reason.

## 8. Short self-check practice

Try these before moving on.

1. A problem asks whether any ID number appears more than once in a long list. What is the real task?
2. Why can `if x in seen:` become expensive when `seen` is a list?
3. Why should thousands of middle `.remove()` and `.insert()` operations make you suspicious?

Suggested answers:

1. Membership or seen-before tracking.
2. Because Python may scan through the list item by item each time.
3. Because repeated middle edits can force lots of shifting and make the solution both messy and slow.
