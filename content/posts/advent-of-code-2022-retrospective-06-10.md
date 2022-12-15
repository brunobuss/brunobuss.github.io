---
title: "Advent of Code 2022 - Day 06 to 10 Retrospective"
date: 2022-12-11
draft: false
#categories:
#- Programming
#tags:
#- AoC
#- GoLang
---

### Day 06

This was a nice one as the part two extended very easily from part one if you used a proper data structure,
in this case avoing a hard coded comparisson of 4 values in the first part.

Another thing that I did was to keep it a linear scan while maintaining a map with the count of the characters
in the window if `k` characters back. The more trivial approach would be to build a set of size `k` for each
possible window. Since `k` was very small for this challenge (4 and 14), there was not much difference in performance
or what :)

### Day 07

Ah! Today's challenge really excited me, mostly because I had not yet used RegExp with Go!

So, I started by doing some unit tests to make sure my compiled regexes were working as intended.
This is by far on of the easiest way to trip up in a problem like this.

After that, the line by line parser was not hard to put together.

One thing that tripped me ugly this time is a well known thing for Go devs I believe:
```go
	mymap := map[string]mystruct
	for k, v := range mymap {
		// Trying to change things inside `v` and realising because its a copy
		// of the value inside the map. Oops!
	}
```

Also had some fun with pointers in Go :)

### Day 08

Day 08 was simple and easy to do. The trivial solution doing 4 scans (one in each direction) from
each cell in the grid would yield a `O(n^3)` time algorithm, which was perfectly fine given the
input size we had to work with (100x100 grid).

I do believe (although I didn't implemented it to verify) that it is possible to bring it down to
`O(n^2)` time by using `O(n^2)` extra space (which is ok too, given we already used `O(n^2)` space to
hold the grid in memory) by doing a left-right-up-down scan and another right-left-down-up scan of the
grid to precompute some values to answer in `O(1)` instead of `O(n)` for each cell if:
  a. Is it visible from outside?
  b. What is the scenic score?
  
But as mentioned before, the small input size allowed the `O(n^3)` solution to work anyway :)

### Day 09

This was an interesting challenge on how the solution from the 1st part if well written would apply
very easily to the 2nd part.

I thought a bit but couldn't find a very good way to simulate a `[direction][k]` instruction in one
step instead of `k` for the first part, which turned out to be a good thing because my gut feeling
tells me it would be a mess to adapt it to the 2nd part.

### Day 10

Oh may, I really liked this challenge! Like day 07 one, I tend to like things that are based on
actually computing stuff :p

This one required us to simulate a very small computer architecture: one register, two possible
instructions and one "output device".

Initially I tried to implement a solution that would simulate each instruction while keeping a
"clock counter" was increased by one in a `NOOP` and two in an `ADDX`. The issue here was
that the logic to detect if something happened after the first but before the second clock of
the `ADDX` started getting more complicated than what it looked like it would need to be.

Because of this I changed to a model where I would simple call `simulateStep()` once for `NOOP`
or twice for `ADDX`, and change the register value after the 2nd `simulateStep()` in the `ADDX`
case.

In the end, I got lucky as I could easily extend my core logic to print as needed to the "output device".
Then, the main challenge was a personal one of trying to read some ASCII-art :p 
