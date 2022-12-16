---
title: "Advent of Code 2022 - Day 11 to 15 Retrospective"
date: 2022-12-16
draft: false
#categories:
#- Programming
#tags:
#- AoC
#- GoLang
---

### Day 11

This challenge join two things that I usually don't like very much:
  1 *VERY* annoying input to read/parse.
  2 Non-sense simulation (unlike the one in day 10 :)
  
In order to avoid having to deal with `1` I went ahead to solve the simulation first here.

The simulation itself was pretty easy for part one and while creating it I defined my structs
which would hold the monkey data, including a function pointer to allow easily representing how each
monkey affected the "worry" level of the item.

After the simulation was done and tested I went to solve the input part. When I saw that the real
input for this challenge was composed of only 8 monkeys ... I just went ahead and manually transformed
the challenge input into a Go struct! :p

The second part was interesting, since the numbers would grow very fast and screw up all the computation.
Not sure if it was possible to solve this using BigInt or something in that direction, but on a gut feeling
I decided to cap the values based on the [LCM](https://en.wikipedia.org/wiki/Least_common_multiple) of the monkeys
div test value, because what was important for the whole simulation is that those tests kept working as intended.

In the end, it was even easier than expected when you see that all div test values are distinct prime numbers,
so I only had to multiply all of them to get my LCM and the cap for the worry numbers. (I wonder if maybe my brain
picked up this fact even before I was aware of it in order to think about the LCM?)

I was happy that I my small knowledge of number theory nudged me toward the right answer ... but could see how
a challenge like this could be kind of frustrating if you're totally stuck on it and never had experienced a
similar question before.

### Day 12

Yes, graph-based challenges! :D

This one being quite simple, based on a grid. A [BFS](https://en.wikipedia.org/wiki/Breadth-first_search) starting
from `S` in order to find the shortest distance to `E` (nothing more fancy was necessary, since every edge has
cost/distance equal to 1).

Part two, you could just reverse the BFS to start at `E` and stop at the first found `a` (or `S`) cell.

Easy, simple and very enjoyable to code :)

### Day 13

Oh boy!

Lets address the elephant in the room: I think it was totally fine if you read the input using something like `eval()`
in Python or a JSON library to deserialize it.

But, writing your own toy parser for something is just too much fun to pass it up, right?! :p

This parser was not that bad to write, a recursive one with a single character look ahead was enough to handle all the
cases. But in order to test it properly, I ended up implementing the serialisation mechanism too just so I could more
easily check if what I was parsing was making sense! (And in the end, it was handy to debug an edge case of the actual
challenge logic.)

The comparison logic was not hard to code, but I think it is very easy to get an edge case wrong. Which was what I did and
unfortunately none of the sample input cases caught it. Where I tripped up and took me awhile to find and fix it (and required
debugging sample pairs of the full input, since it was not clear what was right/wrong): my code was considering that exactly
similar lists to be in the right order. So an input like `[[3], 4]` and `[[3], 1]` would return right order (because of `[3]`)
when it should have said it was the wrong order.

Again, this was *nasty* to find without a test case. This challenge was the one where I spent more time by far since the beginning
but also one where I feel like I had lots of fun, mostly for two reasons:
  - Got to implement my own parser :p
  - Very interesting debugging session :)

### Day 14

This was interesting. On the face value, it looked like it was another simulation problem where you would spawn a new blob of sand,
simulate where it fall, and repeat until you cannot spawn anything more. But from the get go it sounded inefficient enough that I
would end-up having trouble in the part two ...

After sometime thiking about it, I saw that the simulation of a sand being spawn at `(x, y)` could be described by something like:
  - If `(x, y)` is a rock coordinate or already has a piece of sand there, we cannot spawn a sand here.
  - Else if `(x, y)` is a coordinate that would fall into the void, we should return signaling for the logic to stop.
  - Else we could just try to spawn sands in `(x, y+1)`, `(x-1, y+1)`, `(x+1, y+1)`, while keeping a state variable
    for the spaces occupied by sand pieces.
  
The order is important because of the left/right logic in the problem statement, which is one possible edge case if not handled properly.
For example, like in the part one for the sample, we detect falling into the void when going to the left (`(x-1, y+1)`), so we never
get to include the pieces of sand that would fall to the right (`(x+1, y+1)`) from the starting point.

Surprisingly, I found that the changes required for the part two of this problem was very trivial to make and actually made the code smaller!

### Day 15

This was another challenge that I enjoyed solving.

For the first part, since it was concerned only with a single fixed line in the world it was enough to compute the range that each sensor
would cover on that line, mark those positions on the map and count everything that was marked. The edge case I found here was forgetting
to remove a beacon that was covered in this line from the count.

Now for the second part it to find the single coordinate that has an open uncovered spot, since we had to scan all the 4M lines, the method
above would be too slow. So I changed the logic from marking positions in a map to working purely with open-ended ranges representations
(`[open, ended)`) of the covered parts of each line and merging those ranges. After that, any line that had more than one range implied
that we had a gap of coverage in that line and then it was easy to compute the tunning frequency required :)

### Wrap Up

I noticed that for half of the [last entry]() and this one now I mostly talked about the algorithmics aspects of the challenges
and not much more about Go itself. And I really think this is mostly because I'm having such a good time writting these solutions
in Go!

Honestly, Go allowed me to express all my ideas very easily so far. I feel much more comfortable and confident while writting Go
now than I was two weeks ago when I started AoC (Wow! It has been two weeks already?! WTH?!).

I also know that this is all that Go has to offer, there are many aspects of it like goroutines and channels that I don't see myself
using for AoC challenges but I would like to get some more practice with them in a near future too.