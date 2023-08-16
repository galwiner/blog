---
title: "The CLASSIQ Engine: I CAN get some satisfaction"
date: 2023-08-16
math: false
featured_image: "csp-cover.png"
draft: false
---

This post was [originally posted](https://www.classiq.io/insights/the-classiq-engine-i-can-get-some-satisfaction) on the Classiq blog.

Have you ever solved a Sudoku puzzle? It was pretty popular at some point in the early 2000s. For some reason, everyone was solving them all the time. Mind you, this is a time well before smartphones. People just didn't have better things to do. If you haven't heard of it, in a Sudoku, the goal is to fill a 9X9 grid with digits such that in each row and each column, each digit appears only once. This type of puzzle is an example of a Constraint Satisfaction Problem (CSP). These are problems where you have to "fill in the blanks" with an item (e.g., a digit) from a set of possibilities (e.g., the digits 1 through 9) but not break some set of rules (e.g., no repetitions of a digit), and they're more common than you'd think.  

The class of CSP has other, arguably more interesting, instances in the form of games (any crossword puzzle), mathematics [map coloring](https://en.wikipedia.org/wiki/Four_color_theorem), and so on. There's a reason why this whole thing is important to us, though, and it isn't because we want to prove a mathematical theorem. It's a lot more practical than that. At least if you're in the business of quantum algorithm design. 


### Optimization of Quantum Circuits

There are no free lunches, but getting the best bang for your buck is nice. That's the point of optimization techniques: finding how to maximize (or minimize) some problem metric. There are many types of such problems. Sometimes the thing you are trying to optimize takes continuous values, while other times, those values are discrete. Your optimization space can be either finite or infinite, and it can also vary in different ways. You have a set of constraints in CSPs, and you want them all to be met as best as possible. This sub-class of CSPs can be called a CSOP, where "O" stands for "Optimization." 

We want to design a quantum algorithm and make our building blocks "high-level." So we want to work with something other than individual quantum gates. Instead, we'd like to use a function call that is then converted to the correct gate sequence. This step, during which high-level function calls are replaced by gate sequences, is called synthesis. It's not the only thing that happens here, but for simplicity's sake, let's just consider one aspect. When synthesizing, we are constrained mainly by the quantum hardware. We have a finite number of qubits available, an upper limit to the circuit depth we can execute before noise overwhelms us, and so on.

The game we are left with is as follows: Allocate resources (qubit number, circuit depth, etc...) to functions in a way that optimally meets specified constraints. 

 


### Meeting your Goals, but the Ground is Moving

In a simple case, each function call is a "mask," which can be replaced by a fixed gate sequence. You place those sequences one after the other and get a circuit. That would be boring and give little wiggle room for synthesizing. In this case, the sequence of function calls uniquely determines the resulting circuit. Luckily something much more interesting and more difficult is actually happening here.

This is a more interesting problem than you might assume because there is more than one correct implementation for some functions. Different implementations come about for various reasons. One example is due to hardware-dependent implementations. These occur due to different native gate sets in trapped-ions quantum processors to those of neutral-atoms quantum processors (or other qubit types). Another is due to the connectivity map of qubits. These maps specify on which pairs of qubits conditional operations can occur, and they vary between qubit chip architectures (even for a given qubit type). 

Multiple implementations are not just due to differences in hardware, though. One example is that of the adder circuit. This circuit element allows adding the numbers represented by different registers (sets of qubits). This is a fundamental component of any computer, including classical ones. It is equally valuable for quantum computations. Implementing this element has more than one representation as quantum logic gates. The purpose of the adder can be achieved by a [ripple-carry circuit](https://en.wikipedia.org/wiki/Adder_(electronics)#Ripple-carry_adder]) or by usage of the Quantum Fourier Transform (see [this paper](https://arxiv.org/pdf/1411.5949.pdf)). Different implementations have different characteristics in terms of the number of qubits used, circuit depth, number of two-qubit operations performed, and so on. 

### Reduce, Reuse, Recycle

Resources are scarce in computing, whether quantum or otherwise. Unless you are solving really easy problems, you typically run into one of two walls. You either don't have enough speed or enough space. Often you can convert one of these problems into the other, but only sometimes, and it only sometimes helps. 

For this reason, developing methods for making frugal use of resources is an old (and necessary) tradition in computing. Efficient memory management or garbage collection is essential and allows programs to repeatedly reuse the same bits of physical space in a program. Automating these tasks has been the domain of Electronic Design Automation (EDA) software which was a source of inspiration for CLASSIQ’s engine.   

When synthesizing quantum programs, one particularly tricky piece of the puzzle is what to do with auxiliary qubits. Again, let's focus our attention on what the synthesis engine has to do. 

The engine selects an implementation of the high-level function and then "connects" it to the following function implementation. If that function has an auxiliary qubit (or qubits), meaning one that was used and is now irrelevant, it can be reused by another function. If it cannot be reused, and the following function also needs an auxiliary qubit, we will have to allocate a new qubit to that function (which we may not have handy!). 

This reuse of aux. qubits makes the Constraint Satisfaction (and optimization) problem we are trying to solve "non-local," in some sense. What if selecting one implementation (and auxiliary qubit wiring) at the beginning of the circuit, which seemed to be a good idea at the time, forced me into choosing a less suitable implementation later on? One that is overall worse for optimizing my target? You make a choice, and the end goal moves in one direction or another, making you reevaluate your decisions and change them retrospectively. If only real life had given you such liberties. 


### Simple Strategies for Success

There are many ways to arrange functions and allocate resources to each one. The number scales exponentially with the number of functions, as quantum things tend to. That means that by a brute force approach, you won't solve the problem if you have many function calls unless (maybe) you already have a working quantum computer to speed up searching through the possibilities efficiently. You'd still need to store them somehow, so even a quantum computer may not be enough!

There are many ways to tackle this difficult problem. The first approach, which is slow but at least methodical, is by [backtracking algorithms](https://en.wikipedia.org/wiki/Backtracking). This is an incremental (recursive) way to build the circuit layer-by-layer. To improve on this, there are better algorithms, some based on clever heuristics. We will say more on such techniques in future blog posts. 


### What Next? 

Today we got a small glimpse into the world of CSP and how it applies to the problem of quantum algorithm circuit synthesis. Or, as we call it at CLASSIQ: bread & butter. 

We learned about the problem setup and what could naively be done to start tackling it. But the CLASSIQ engine takes these ideas much further. Beyond applying more intricate and clever ways to make block implementation selections quickly and efficiently, there are extensions of the problem due to the specific challenges brought about by the quantum nature of the circuits we need to build. 

How would you solve a CSOP when you can perform mid-circuit measurements? How should considerations such as coherent and incoherent noise be integrated into a synthesis engine? How can information processing in a hybrid classical-quantum computation workflow feed, at execution time, into circuit synthesis or re-synthesis? These are all difficult and fascinating questions. In future posts, we will dive deeper into the world of circuit synthesis and the algorithmic challenges it poses. 
