---
title: Tutorial 1: What Lists Are Actually For
permalink: /tutorial-01/
layout: default
next_title: Tutorial 2: A List Crime Scene
next_url: /tutorial-02/
---

{% include chapter-nav.html position="top" %}

If you already know Python lists, it is easy to use them for almost everything.

That is normal. Lists are usually the first data structure students learn well, so they become the default choice.

But this series is about better judgement, not just more syntax.

Before we talk about when lists are a bad fit, we need to be fair to them first.

Lists are not the enemy. In many problems, a list is exactly the right tool.

The real question is:

**What job does a list do well?**

For competitive programming students, the answer is usually this:

- a list is good when **order matters**
- a list is good when you need **indexing**
- a list is good when **duplicates matter**
- a list is good when you want to **sort values**
- a list is good when the data is naturally a **sequence**

These ideas usually come together. If a problem treats the data as an ordered sequence, indexing often matters too. If you sort the data, order matters again. If duplicates must stay in that sequence, a list becomes an even better fit.

That is why lists are so tempting. They solve many early problems well, so students start to think of them as the default answer. Sometimes that instinct is correct. Sometimes it is not. This tutorial is about seeing the cases where it really is correct.

So the big signal is not just “I know how to write square brackets”.

The big signal is:

**Does this problem treat the data as an ordered sequence?**

That is the central lesson of this tutorial.

## 1. Lists are good when the data is an ordered sequence

A list remembers the order in which items appear.

That makes it useful when position matters, or when you need to process values from left to right.

Example: storing scores in the order they were entered.

```python
scores = [12, 18, 18, 9, 15]

print(scores)
print(scores[0])
print(scores[-1])
```

Output:

```python
[12, 18, 18, 9, 15]
12
15
```

This is an ordered sequence of numbers, not just “some numbers”.

That one idea already brings several list strengths with it:

- you can keep the original order
- you can use indices
- you can compare neighbours
- you can keep duplicates in place

If a problem says “read the numbers in order”, “print the third value”, or “scan from left to right”, a list already matches the job well.

That is an important point for the whole series. Choosing the right structure does not mean always replacing lists with something else. It means noticing when the problem really is sequence-shaped.

## 2. Lists are good for indexing

A list lets you get an item by position.

```python
letters = ["a", "b", "c", "d", "e"]

print(letters[2])
print(letters[4])
```

Output:

```python
c
e
```

In contest problems, indexing shows up all the time. You might need to get the first or last element, update the value at position `i`, or compare neighbours.

Those are all position questions. A dictionary is strong at key -> value lookup, and a set is strong at membership, but neither one is the natural tool when the problem keeps talking about the 1st value, the next value, or the value at index `i`.

```python
heights = [150, 147, 152, 149]
heights[1] = 148

print(heights)
```

Output:

```python
[150, 148, 152, 149]
```

If the problem talks about positions, neighbours, first, last, or “the value at index `i`”, a list is often a natural choice.

## 3. Lists are good when duplicates matter

A set removes duplicates. A dictionary counts or links things by key.

A list does something different: it keeps repeated values exactly as they appear.

```python
rolls = [4, 6, 4, 2, 6, 6]

print(rolls)
```

Output:

```python
[4, 6, 4, 2, 6, 6]
```

In many problems, duplicates are part of the data:

- test scores
- dice rolls
- card values
- daily temperatures
- queue events

If the problem needs the full sequence, including repeated values, a list makes sense.

## 4. Lists are good for sorting

Lists work naturally with sorting.

In competitive programming, sorting is a very common step. Sometimes the whole problem becomes easier after sorting the values first.

```python
numbers = [8, 3, 5, 3, 10]
numbers.sort()

print(numbers)
```

Output:

```python
[3, 3, 5, 8, 10]
```

Notice two useful things here:

- the values are now in order
- the duplicate `3` is still there

Example: checking the smallest and largest values after sorting.

```python
times = [42, 35, 51, 35, 47]
times.sort()

print(times[0])
print(times[-1])
```

Output:

```python
35
51
```

If a problem asks for sorted output, smallest and largest values, or neighbour comparisons after sorting, a list is a practical tool.

## 5. Lists are good when order and sequence shape are part of the problem

Sometimes the best clue is the shape of the data itself.

If the data is naturally “one thing after another”, a list is often the most sensible choice.

Examples:

- the moves in a game
- the temperatures over seven days
- the passengers entering a bus stop by stop
- the answers in the order a student wrote them

```python
moves = ["L", "L", "R", "U", "D", "R"]

for move in moves:
    print(move)
```

A list works well here because the order is part of the meaning.

You often read a whole line of values into a list, then process them in order. That is simple, and often correct.

Sometimes the whole solution stays list-based. Sometimes the problem starts with a list and later needs a set or dictionary for a different job. That is fine too.

The important point is that a list is often the correct place to begin when the data starts as an ordered sequence.

## 6. A quick contest-style example

Suppose a problem says:

> Read `n` race times. Print them in sorted order. Then print the difference between the fastest and slowest time.

A list is a strong fit immediately.

```python
times = [19, 23, 18, 21, 18]
times.sort()

print(times)
print(times[-1] - times[0])
```

Output:

```python
[18, 18, 19, 21, 23]
5
```

Why is a list the right tool here?

- order matters after sorting
- duplicates matter because two runners can have the same time
- indexing is useful for the first and last values
- the input is naturally a sequence

## 7. What this tutorial is not saying

This tutorial is not saying that lists solve every problem.

Later tutorials will show cases where students use lists for membership, counting, or seen-before tracking when a different structure would fit better.

But if you only hear “lists are bad”, you end up with a different problem: you stop seeing when lists are genuinely useful.

That is why this first tutorial matters.

The point is simpler:

good problem solving is not about avoiding lists.

It is about using lists for the jobs they are actually good at.

## Self-check

Try these before moving on.

1. A problem gives you the finishing order of runners and asks for the runner in 3rd place. Which list strengths are helping?
2. A problem gives you five card values, repeated values are allowed, and then asks you to sort them. Why is a list a better fit than a set?
3. A problem gives you daily temperatures in order and asks whether a temperature is higher than the day before. Why does indexing matter?

Suggested answers:

1. Order matters, and you can access a position by index.
2. Because duplicates must be kept, and the values still need to stay in a sortable sequence.
3. Because you need to compare neighbouring positions in the sequence.

{% include chapter-nav.html %}
