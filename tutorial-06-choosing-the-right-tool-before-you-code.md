# Tutorial 6: Choosing the Right Tool Before You Code

## 1. What this tutorial is about

This is the final tutorial in the series.

The goal is not to teach a brand new structure.

The goal is to build a habit:

**before you write code, stop and identify the real job.**

You already know the basic syntax of:

- lists
- sets
- dictionaries
- `defaultdict`

But syntax is not the same as judgement.

In competitive programming, students often lose marks, waste time, or risk TLE not because they cannot write Python, but because they choose the wrong structure too early.

That is the habit the whole series has been building toward.

## 2. A familiar student instinct or common starting point

A very common starting point is:

**“I know lists best, so I will start with a list.”**

That feels safe, but it can lead to trouble.

Students sometimes use a list when the real job is:

- membership
- counting
- lookup
- grouping
- tracking seen values

Then the code starts to grow in awkward directions:

- repeated `if x in my_list`
- parallel lists that must stay lined up
- repeated `.remove()` and `.insert()`
- long setup code just to count or group values

A lot of this code can still work. That is what makes it dangerous.

The problem is not always correctness. Often the problem is that the code is harder to read, harder to reason about, slower than it needs to be, and more fragile under contest pressure.

## 3. What goes wrong

When students choose a structure too quickly, they often choose based on familiarity instead of fit.

That creates two kinds of problems.

First, the code becomes awkward.

Second, the code becomes risky in contests because it is more likely to hide repeated work, create logic mistakes, and waste debugging time.

Strong programmers pause before they code.

They do not only ask:

**Can I code this?**

They ask:

**What operation matters most in this problem?**

That question usually leads to a better structure choice.

At first, it is fine to use a checklist. Later, with practice, you start recognising the shape of the problem more quickly.

## 4. A compact decision guide

Before you write code, run through this checklist:

- If order, neighbours, indexing, or sorting matter, start by thinking **list**.
- If the main question is “is this already here?” or “have I seen this before?”, think **set**.
- If the main question is “how many times?” or “what value belongs to this key?”, think **dictionary**.
- If one key needs to collect many values and you keep writing “if missing, create a default value”, think **`defaultdict`**.

If you like, turn that into quick mental questions:

- Do I need order?
- Do duplicates matter?
- Am I checking membership?
- Am I counting?
- Am I linking keys to values?
- Am I grouping values?

You do not have to ask them in a perfect order every time. The point is to pause long enough to identify the main job before the code starts pulling you in the wrong direction.

There is one rescue question that matters a lot:

**Am I writing awkward list code because I chose the wrong tool?**

If your code is full of repeated `in` checks on a list, parallel lists, repeated middle edits, or repeated missing-key setup, stop and rethink the structure.

Also remember this:

you do not always have to choose exactly one structure for the whole problem.

A contest solution might read input into a list, use a set to track seen values, and use a dictionary to count something else. That is normal.

The goal is not to force one structure to do everything.

The goal is to let each structure do the job it suits best.

## 5. Worked example

Here is a small contest-style problem:

> Read `n` scores in order. Print `"YES"` if any score appears more than once. Then print the number of different scores.

Do not ask only, “What syntax do I know?”

Ask what each part of the problem needs.

- Reading the input in order suggests a list.
- “Have I seen this score before?” is a membership question, so think set.
- Counting different scores can also be handled by that same set.

That is the kind of thinking you want to practise.

So this is really a **list plus set** problem:

- the list stores the input sequence
- the set tracks seen scores and distinct scores

```python
scores = [12, 18, 12, 9, 15]
seen = set()
has_repeat = False

for score in scores:
    if score in seen:
        has_repeat = True
    seen.add(score)

print("YES" if has_repeat else "NO")
print(len(seen))
```

The list is useful, but it is not the whole story.

A problem can begin as sequence input and still turn into a membership or counting problem once you inspect the operations.

## 6. Common patterns

Here are some short scenarios where the right starting tool should become clearer before you code.

### Scenario A: Print the values in sorted order, then compare neighbours

Best starting tool: **list**

Why: order, sorting, and neighbour checks matter.

If the problem keeps talking about first, last, next, or sorted order, that is list language.

### Scenario B: Decide whether any ID number appears twice

Best starting tool: **set**

Why: this is a seen-before and membership problem.

The question is not “where is it in the sequence?” It is “have we seen it already?”

### Scenario C: Count how many times each word appears

Best starting tool: **dictionary**

Why: each word needs a count.

That is a key -> value relationship: word -> count.

### Scenario D: Store every position where each number appears

Best starting tool: **`defaultdict(list)`**

Why: each number maps to a list of positions.

That is grouping, not plain sequence storage.

These are small decisions, but they make solutions cleaner and safer.

## 7. Common mistakes

### Mistake 1: Choosing the structure you know best instead of the one that fits best

Familiarity is not the same as suitability.

### Mistake 2: Starting to code before naming the real task

A short pause at the start can prevent a long rewrite later.

### Mistake 3: Treating all problems as sequence problems

Some problems are really about membership, counting, lookup, or grouping.

### Mistake 4: Ignoring awkward code as a warning sign

If your code feels like it is fighting you, pay attention.

Awkward list-heavy code is often a clue that the data structure choice is weak.

If the code feels awkward, step back and ask what operation the problem actually cares about.

A working solution is not automatically a good solution. In contests, cleaner structure choices save time both in runtime and in debugging.

## 8. Short self-check practice

Try these before you finish the series.

1. A problem gives a sequence of race times. You must print the times in sorted order, then print how many different times there were. Which structures would you choose, and why?
2. A problem gives moves on a grid and asks whether the robot ever visits the same square twice. Which operation matters most, and what structure should that suggest?
3. A problem gives many pairs `(value, position)` and asks you to print all positions where each value appeared. Why is a plain list awkward here?
4. If your code uses several parallel lists or repeated `if x in my_list`, what question should you ask yourself?

Suggested answers:

1. A list and a set. The list handles the sequence and sorting. The set handles distinct values cleanly.
2. Membership or seen-before tracking. That should suggest a set, often with tuples like `(r, c)`.
3. Because one value may need to link to many positions, which is dictionary-shaped rather than sequence-shaped. `defaultdict(list)` is a clean fit.
4. “Am I using awkward list code because I chose the wrong tool?”
