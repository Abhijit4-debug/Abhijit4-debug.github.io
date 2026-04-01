---
title: "Bottom-Up Is the Worst Way to Teach Dynamic Programming"
summary: 'Maybe it is just me, but starting with a filled table and working backwards has never been how my brain wants to approach these problems.'
date: 2026-03-31
math: false
toc: false
tags:
- dynamic programming
- algorithms
- teaching
- interviews
---

Maybe it is just me, but I think bottom-up is genuinely the worst way to introduce someone to dynamic programming.

I am prepping for interviews right now and every resource I open, every textbook chapter, every YouTube video, every LeetCode editorial, leads with bottom-up. Here is a problem, here is a table, here is how you fill in the table from the base case upward, here is the recurrence that governs how each cell depends on previous cells, now your answer is in the last cell. And I can follow along and reproduce it for that specific problem, but the moment I get a new problem I am staring at a blank screen because the entire framework I was given starts with "construct the right table" which is the hard part, not the easy part.

I spent a genuinely embarrassing amount of time thinking I just did not get DP. Like maybe my brain was not wired for it, maybe I needed to grind more problems until the patterns clicked. And then at some point I started approaching problems top-down instead, just writing a recursive function that described what I was actually trying to compute, and adding memoization so it did not redo work, and suddenly things made sense in a way they never had before.

The thing that changed was not some insight about DP itself, it was just that top-down matches how I actually think about problems. You define what your function means, like f(i) is the best answer considering elements from index i onward. Then you think about what choices you have at each step and what smaller subproblem each choice leaves you with. Then you cache it. That is the whole thing. You do not need to know the shape of the table upfront or what order to fill it in, because the recursion figures out the ordering for you.

Bottom-up requires you to already understand the full structure of the problem before you write anything, the subproblems, the dependencies, the dimensions, all of it. That is the end state of understanding a DP problem, not the beginning, and teaching it first feels like showing someone the final draft of an essay and asking them to work backwards to the outline.

I get why bottom-up is what gets taught though, it is faster in practice because you avoid recursion overhead and stack depth issues, and you can sometimes optimize memory by dropping dimensions of the table. Those are real advantages. But they are optimization concerns, and in every other context we tell people not to optimize before they understand what they are building.

I think about this a lot because I keep running into people who have the same experience, they feel like DP is this mystical thing where you either see the table or you do not, and then you show them the top-down framing and it clicks almost immediately. Which makes me wonder how many people bounced off DP entirely just because the way it was presented did not match how they think, and how much of DP's reputation as this uniquely hard topic is really just a pedagogy problem.

Maybe I am wrong and bottom-up really is the better starting point for most people and I am the outlier here. But I do not think I am, and I wish more resources would lead with top-down and treat the conversion to bottom-up as the mechanical optimization step it actually is rather than the default way to think about these problems.
