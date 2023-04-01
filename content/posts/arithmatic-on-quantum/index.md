---
title: "Arithmetic on Quantum Computers"
date: 2023-03-13T17:16:42+02:00
math: true
draft: true
---

# How do you calculate 1+1 on a quantum computer?

## Uri Levy's question
There's something about being part of a group that's wonderfully transformative. You drink coffee with some people, day in, day out, for a few years, and start speaking a common language. You work with them, commute with them, and walk past them in the halls. And you end up being like them or at least trying to be. 
Working in the Weizmann Institute's (WIS) complex system department was in a very transformative environment. There are many ways in which this place has a language of its own. There's the science, of course. It's the atoms, ions, lasers, magnets, resonators, and nonlinear crystals. It's catching the end of someone's sentence when walking by "...and that's just adiabatic elimination once again!". But there's more to that than just that. It's also about how people reason about the world in general. How they explain themselves and question others. 
Encountering someone who thinks very clearly can be magical. I tried back then, as I do now, to emulate such figures. The school of thought of Nir Davidson, Ofer Firstenberg, Roee Ozeri, and others. It's a school whose motto is always trying to distill an idea to its simplest and most condensed form and (usually) doing so kindly. 
Another of these figures is Dr. Uri Levy. A physicist who had roamed those halls as a young student. He then pursued a career in physics in industry, only to return once more as a moth to the flame. Uri has a way of asking questions that is like Socratic dialogue. They are delivered with quiet honesty but tend to find weak spots in the argument, like a stinger missile hitting a Russian tank. 
After leaving WIS's comforts, I started working on quantum computers. Uri was curious and called me one day to ask, "So, I know quantum computers will break cryptography. But how do I even use one to calculate 1+1"? I promised Uri an answer for a while and kept putting it off. The rest of this post, which started with a long-winded (and superfluous) introduction, aims to address this question and to do so in the spirit of the complex systems department. 

## Number types and binary arithmetic

Until the early 20th century, "A Computer" was a person doing the work of calculating things. Today, when we say this word, we mean an electronic device that performs computations using something stored on it **physically**. This information, a list of symbols representing ones and zeros not so dissimilar to lines cut into a clay tablet, can be implemented in many ways. For example, they can be on flexible plastic tape coated by a thin layer of iron. Segments of this layer can then be magnetized, with the magnetic North pointing one way or the other. Manipulating these stored bits of information, done by flipping the direction of the magnetic poles, is how we change what is stored on the tape.

## Counting with cogs

Our brains are great at remembering every lyric of an album we listened to as a teenager. But we're not good at keeping track of too many digits, especially when out of context. Some people can recall Pi to fantastic precision, but that's a feat achieved by specific and targeted practice. A typical person only has a working memory of about 7 digits, but for me personally, it's much less. This is at least part of why children use fingers when learning basic arithmetic. Keeping track of the digits, with their digits (which is why we call them digits, to begin with!).
So external memory is essential when performing arithmetic. Still, to apply a mechanical process to do mathematics for us, we need to add some logic. You can do that in as many ways as your imagination allows, but using machines is an excellent way. 

I've recently visited the Musée des Arts et Métiers (Museum of Arts and Crafts) in Paris. In typical Parisian fashion, it's this beautiful historic building. The spacious and bright exhibition halls include a converted gothic church which, as designed, evoke a gasp as you enter. Instead of a pulpit and religious iconography, a full-size Vulcain rocket engine (the kind used on the Ariane 5 vehicle) is as big as a small bus and at least as prepared to leave this earth as the biblical angel it replaced. 

The museum holds all manner of historic scientific instruments and is well recommended. However, one relic I was particularly interested in is the Pascaline, Pascal's mechanical calculator. 

![img.png](pascalino.png)

This machine has a panel with several dials, each inscribed with the digits 0-9. Like when writing a number on paper, each dial position encodes a different decade (units, tens, hundreds, and so on).
This enables you to input a number into the calculator, which is a mechanical memory. Using it frees the user's mind from remembering the first number of your computation. Having input a first number,  the dials are repeatedly turned to input the number to be added or subtracted. The display will then show the result of the computation. Addition can be repeated multiple times to achieve multiplication. By the same token, subtraction can be repeated for division (with remainder). You do need to keep track of the number of times you added or subtracted manually, though (as far as I could tell).

Counting with a dial is nothing fancy. If I set the dial to, say, 2 and then turn it by 2 positions, it will show the value "4". I've successfully performed the computation 2+2=4. However, if the dial was at 9 and I now try to add 2, I will get "clock arithmetic": 2+9=1. This is the result modulo 10, my counting basis. This is not how multi-digit numbers are added, though. We were taught, at a very young age, that we need to "carry the 1". So when we add two digits, and the result is larger than 10 (or whichever number basis you may be working in), you must pass the overflowing digit to the following position to the left.

### Half adder and full adder

A half-adder is a device implementing this behavior, where the overflow digit is passed on (more commonly referred to as "carried over"). A half-adder takes two input numbers and outputs a sum and a carry digit. Two inputs, two outputs. More is needed for a functional adding machine, as you also need a carry-in digit (to check if the previous position has overflowed). A device with this more complex behavior having **three* inputs (two numbers + carry-in) and two outputs (sum and carry-out) is called a full adder. 

In the Pascaline, under the hood, a set of full adders is implemented with cogs. Each cog meshes with the ones before and after it such that a full rotation of the previous cog drags the next one by one position. Doing this smoothly is the heart of the innovation introduced here. 

## Calculating with qubits


### The quantum full adder circuit

It seems that all that remains to be explained is how to make a quantum full-adder. One which can be implemented with atoms, photons, or trapped ions. Here's an example implementation of such a circuit:

![adder_classiq](adder_in_classiq.jpg)

This is a quantum circuit diagram. Each line is a quantum register, which in principal can represent multiple qubits but in this example each one is a single qubit, so what we are looking at is a 4 qubit circuit. The quantum logic gates operating on the qubits are all controlled-not (also called controlled-Z) gates. The solid dots are the control qubits and the open circle with a plus sign is the target qubit. Note there can be multiple controls qubits. I could go through the logic 

I used The truth table for a multi-controlled X gate: 



What if I want to add registers with more? 


![adder_classiq_3qb](classiq_adder_3qb.jpg)

Note, importantly, that doing so will get you something more involved than a classical-logic implementation. 

### Uncomputation and garbage collection

In principle that's true. At least if all you want to do is a single computation that does not feed into the next algorithmic step. That would be a very bad use case for a quantum computer, though. Unless your arithmetic step is the last one in the quantum circuit, it feeds into other algorithmic building blocks. If that is the case, you will end up with some unavoidable house keeping. 

If we have uncomputation [wiki](https://en.wikipedia.org/wiki/Uncomputation). 


Like the encoding of digits as positions on a set of cogs, or as on the orientation of magnetic segments on a piece of tape, 


## Imagined Qubit architectures

Even with additional sophistication, we should get 1+1 on a quantum computer out of the way. To do so we should select a specific qubit architecture. 
I don't feel like going into the nuts and bolts of any qubit in particular (in this post). So let's invent a qubit type. It's not dissimilar to existing qubit types but if I don't name it I don't have to explain anything real, like resonance frequencies or interaction rates or other things of this sort. Our imagined register of qubits will be made from a series of potential wells, each containing one spin in its ground state. We can flip the direction of the spin using our control electronics. We can also create a bias (electric) field such that spins want to tunnel out of their well and go to the next well. 




