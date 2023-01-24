---
title: "Advent of Code 2022 - Day 21 to 25 Retrospective"
date: 2023-01-24
draft: false
categories:
- Programming
tags:
- AoC
- Go
---

As a reminder, you can find links to the previous parts at the end of this post and the 
Github repo with my solutions in Go [here](https://github.com/brunobuss/adventofcode-2022-go)
  
### Day 21

Day 21 was a clear and simple problem. The first part consisted basically on reading the input and building the
in memory binary tree and computing the root node.

The second part was quite interesting! We had to compute the value of one of the leaf nodes (`humn`) such that the two
children nodes of root would have the same value.

My solution was:
  1. Find the path from that special leaf to root.
  2. Go backwards on the path doing the reverse operation accordingly.
  
The only thing that I had not thought on advance was the fact that two of the operations (subtraction and dividing) would
have different "reverse" if the special leaf was coming from the left or right child.

### Day 22

Oh my god, this challenge gave me nightmares for 2 days after solving it.

The first part was ok, mostly a non-trivial wrapping to implement. But the second one, geez...
What can I say, I used a bunch of post-its to origami a bunch of cubes. In the end, my solution (how to wrap the movement)
was quite hardcoded for my particular input.

I came back to the problem after a few days and I think it should be possible to find/compute the folding of the cube (so
it would work on any input) and adjust all the movement wraps... but honestly, I don't want to see anymore cubes in front
of me for 1 month.

(Update: It's 24/01 today and I still don't want to see any more cubes xD)

### Day 23

This was an easy simulation. The only thing that tripped me up hard was that I mis-read the first part statement:
```
How many empty ground tiles does that rectangle contain?
```

My brain totally replaced `empty ground tiles` with the whole area of the rectangle and I was almost pulling the hair
out of my head after debugging it for quite some time to figure out why my simulation was not working as intended!

### Day 24

This was a nice problem!

I think the key here is to notice that the valley, because of the cyclic nature of the blizzards, will come back to the initial state
after LCM(width, height) of the valley. So I precomputed all the possible board states and saved them into a map for easily querying later.

Then, you can do a backtrack with memoization in order to find the shortest path to exit. You could mark visited states, but I was lazy and
just capped the backtrack to a depth of a thousand steps to see if it would work ... and it worked :p

The second part was very easy after implementing the initial one. You just had to reverse the start and finish coordinates.
And if you precomputed the board states like I did, some module maths to get the correct initial board state for each of the journeys :)

### Day 25

I really liked this problem, it was quite different from what we have been doing so far!

My initial approach was to convert the snafu numbers to integers (quite easy), sum the integers (very easy) and convert it back to snafu ...
I really didn't expect to get totally stuck on doing the last part! I honestly don't know what I was getting wrong ...

After some time trying to correctly convert an integer back to snafu, I felt like I could instead actually just sum the numbers in snafu format!
And this was actually how I managed to solve the problem :)

### Wrap Up

Oh wow, my first AoC in which I finished all the problems! And even managed to finish each one on its respective day!

It was loads of fun and now I'll wait for the 2023 edition! :)
