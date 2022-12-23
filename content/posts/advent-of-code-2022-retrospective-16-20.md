---
title: "Advent of Code 2022 - Day 16 to 20 Retrospective"
date: 2022-12-23
draft: false
categories:
- Programming
tags:
- AoC
- Go
---

As a reminder, you can find:
  - Part one with days 01 to 05 [here](/posts/2022/12/07/advent-of-code-2022-day-01-to-05-retrospective/)
  - Part two with days 06 to 10 [here](/posts/2022/12/11/advent-of-code-2022-day-06-to-10-retrospective/)
  - Part three with days 11 to 15 [here](/posts/2022/12/16/advent-of-code-2022-day-11-to-15-retrospective/)
  - Github repo with my solutions in Go [here](https://github.com/brunobuss/adventofcode-2022-go)
  
### Day 16

This is a classic type of problem to be solved with [Dynamic Programming](https://en.wikipedia.org/wiki/Dynamic_programming)
or [Backtrack](https://en.wikipedia.org/wiki/BackTrack) with [memoization](https://en.wikipedia.org/wiki/Memoization)
(which are equivalent solutions).

Basically my state (either to index the memoization map or the DP matrix) was composed of how much time left,
where the agent was and the state of the valves. The base case was when we ran out of time (time left = 0), we
would know the best to do from that point is 0.

Even so, the combination of all possibilities can grow quite a lot. But if we notice some details, it makes the
search space of the problem much better:
  - Valves with flow = 0 are basically useless. We should not be concerned with them at all, since it doesn't
    help us to increase our total flow rate. Not caring about opening them reduces the valve state space (since
	only 15 of the 60 valves had flow > 0 in my input for example).
  - We really don't care about wandering around the map, so our next move should always be towards the next valve
    we want to (try) to open. This also really helps to reduce to reduce the search space, since we don't care
	at all for being in places where there is not a valve with flow greater than 0.
	
In order to make the 2nd point faster, I precomputed an all-pairs min distance using
[Floyd-Warshall](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) at the beginning, so I could easily
"move" to the next valve I wanted to open.

Part one done, the second one was quite interesting too. My first attempt was to extend the state to include both
agents (you and the elephant) and trying to map the movements of both of them at the same time. The code got very
messy and the search space exploded and I'm not sure it was feasible.

So after noticing that each agent would interact with a disjoint set of valves, I moved to compute all the possible
partitions of the set of valves (actually half of them, because they would be symmetric) and use the first part solution
to get the best one agent can do with one part and the best another agent could do with the remaining and sum them.
Do this for all possibilities and get the maximum.

I found this problem quite challenging, but I really enjoyed it because it included a bunch of concepts that I liked to
work with :)

### Day 17

Today part one was a bliss, because I love to do game-logic-like coding and Tetris is a cool one to do :D

While solving part one, my brain was already thinking what part two would bring ... and I was thinking like
rotations, disappearing lines, ... Nothing prepared me to what it really was xD

Simulating a trillion pieces! Geez, well ...

The first hunch I got came from the fact we had a small set of shapes that would cycle in the same way and a
set of pushes to left/right that would also cycle too. This led me to believe that at some point we would start
cycling SHAPE and position in the push left/right.

I'm going to be very honest: I'm still not sure 100% why all this works, but I managed to write something to find
a cycle within it. I even went ahead and added more to that state, to try to make sure the cycles were valid, like
adding how much the top of the stack had increased and the offset from the spawn point to where the piece would come
to a rest.

Another edge case was finding a cycle that beings to repeat but doesn't continue long enough, so it was not valid.
So I let the cycle repeat at least twice before declaring it good enough. (Lucky me, the cycle size was not very big,
~2k).

But again, I'm not sure I can explain very well why everything works :|

### Day 18

Ok, after the previous two days I found this to be actually quite nice and easy to solve... which was great, since
the last two took quite some time.

Anyhow, part one was pretty easy and I could compute it as I was reading the input. Basically each new cube adds 6
surfaces to the total count (as it has 6 sides) and then we remove 2 surfaces for each cube that we already processed
that touches the new one.

Second part I initially tried to solve by "shooting a ray" from each face of each cube and seeing if it would touch
another cubelet no not. I missed the fact that it could be non-[Convex](https://en.wikipedia.org/wiki/Convex_polygon),
so it would report external faces as internals.

Then I pivoted to keeping track of min and max `x`, `y` and `z` coordinates of each cube in the input and use it
to create a huge bounding cube with some extra space. Then I would fire a search from the outside in order to map all the
external spaces. This allowed me to fix my logic above.

(And now while writing about it, I just realised I could have ditched this ray-shooting approach and just count the surfaces
I found with my external search :)

### Day 19

This challenge was not that hard, but I spent quite some time on it after solving it!

Just like day 16, it was screaming DynamicProgramming/Backtrack+Memo. And implementing it this time was easier too.
In the end I managed to be the 2170th person to pass part two, I was pretty happy.

Now, what I noticed is that my solution was taking 40 Gigabytes of memory during runtime for part 2!
I knew my 64 GB upgrade I did two years ago would finally pay off! :p

(Fun fact, it would consume 40 GB in the sample input, but less in the real/full input!)

But I couldn't leave it like that, so I spent quite some time looking at cases that I could shave from the search space.

The main aspects I noticed that helped me shave quite some memory was that in many cases we would not need to build certain
types of robots. The most obvious is that if we have enough stockpile to support producing geode robots until the end, just
produce them and mine the most geodes. Another one was that since we can only build one robot per turn, there were many spots
were it wasn't needed to even try to build certain types of robots (for example, if the we have 5 ore robots and none of our
robots cost more than 5 ore, we don't need to try to produce a sixth one).

In the end my solution now takes 1 GB still, but much better huh?! I was very happy by the end of this bit scrubbing session too :)

### Day 20

This was a nice and different problem from what we have been seeing lately.

The first thing that I noticed was the size of input - 5000 elements - which made me believe that just simulating all the swaps
(`O(n^2)`), if you would remember to cap the swaps to not go around the list more than once, would be good enough (and it was).

(Note: I really appreciate that in Go I could swap elements without having to use a 3rd variable - and no xor trickery :)

I think the most challenging part was keeping track of where everything was moving to from the initial position, so you could
easily simulate the shifts in the order they were required.

The one thing that tripped me up was that we could have repeated elements in our list, which confused my map. But this was easy to spot.

Part two was still easy if you the first one was implemented correctly, mostly the part where we capped the number of shifts to be at
most the size of the list by taking the modulo of the required shifts by the size of the list.

### Wrap Up

Not much to say besides that this last batch got way harder than before in general! Still, loads of fun :)

See you next time for the last part with the final 5 days! o/

