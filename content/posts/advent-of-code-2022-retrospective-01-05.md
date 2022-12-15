---
title: "Advent of Code 2022 - Day 01 to 05 Retrospective"
date: 2022-12-07
draft: false
#categories:
#- Programming
#tags:
#- AoC
#- GoLang
---

This is a small retrospective and some notes that I took while doing [the
Advent of Code 2022](https://adventofcode.com/2022/).

For this year I decided to use [Go](https://github.com/brunobuss/adventofcode-2022-go).
Unlike [last time](https://github.com/brunobuss/adventofcode-2021-rust) where I went with Rust to learn
the language (and had lots of fun doing it), I have used a bit of Go during my ~6 month rotation into
GKE SRE while at Google. So even if I'm not totally new to the language, I'm definitely not used to
write idiomatic Go anyway and I hope to improve this by the end of this year's AoC :)

### Day 01

As usual, first day of AoC is always a good opportunity to check if everything
it set-up correctly in my environment and brush up the basics of input reading/parsing.

For part one, we only had to parse groups of ints separated by an empty line, sum them
and return the highest sum. This was a matter of parsing the input and keeping a variable
with the highest group found so far.

On the second part, the requirement was slightly extended to get the 3 highest sums and
sum them. If the input was *very* big (in comparison with the top *k* we wanted), we could
use some manual book-keeping or a [min heap](https://en.wikipedia.org/wiki/Binary_heap) with max
size 3 for an `O(n)` time solution with `O(1)` space ... but since the input was not big enough,
stashing everything in a list and sorting it was enough to solve it (with `O(n*log n)` time and `O(n)` space).

### Day 02

For the first part we had to check which side own a *Rock, Paper, Scissors* game and compute a
simple score based on the outcome plus what your side played.

The second part required a specific outcome, so based on the opponent play you had to choose yours
and pass the game to the function you created in part one to compute the score.

Another very simple challenge overall. It was possible to make the whole solution way more compact
(less lines of code, more maths based than switches to map all the cases) than what I did, but I
went for the easiest and clearer one this time :)

### Day 03

The first challenge that is probably much easier to solve if you think with the appropriate data
structure. First part is a set difference computation, while part two is a set union of 3 sets.

### Day 04

Another simple but very nice challenge, that required only (properly heh) implementing some range
operations.

I need to confess, that while reading the part one problem description my brain was already
jumping very much ahead and thinking of more difficult problems for part two than what it
actually required :)

The range operations that we had to implement were not complex, but it's very easy to get a
comparison wrong in them and then make it hard to spot (disclaimer: might be based on previous
experience *:p*). Given that, I took this to brush off my unit testing skills in Go and write
a nice suite of cases.

Even got to play around with test coverage, which was quite disappointing since 90% of my solution
was not the range operations themselves, but the main() with input reading and output writing xD.

### Day 05

One more challenge that gets us playing with basic data structures. The input looked nasty to parse
in a first glance, but when you notice that everything is aligned and has a predictable offset, it
becomes easy.

For the crates moving part, I was going to simulate one by one but decided to use the Go super powers
of slices and reverse the `x` crates that had to be moved in place and just move them over to the new
stack. What a surprise it was when the part two of the problem only required to skip the reversing! :D

On the downsides, I was a tad disappointed that there is nothing in Go standard library to just reverse
a slice/array and one needs to write something along the lines of:
```go
func reverse(stack []byte) {
	for i, j := 0, len(stack)-1; i < j; i, j = i+1, j-1 {
		stack[i], stack[j] = stack[j], stack[i]
	}
}
```

But other than that, it really feels good/flexible to work with Go slices.

Also tripped up in some indexing, as I initially forgot to transform the 1-based indexing from the input
into 0-based that I was using for my solution. One of the most famous off-by-one errors around :)
