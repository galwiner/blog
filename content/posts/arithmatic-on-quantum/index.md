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
Another of these figures is Dr. Uri Levy. A physicist who had roamed those halls as a young student. He then pursued a career as a physics in industry, only to return once more as a moth to the flame. Uri has a way of asking questions that is like Socratic dialogue. They are delivered with quiet honesty but tend to find weak spots in the argument. Like a stinger missile hitting a Russian tank. 
After leaving WIS's comforts, I started working on quantum computers. Uri was curious and called me one day to ask, "So, I know quantum computers will break cryptography. But how do I even use one to calculate 1+1"? I promised Uri an answer for a while and kept putting it off. The rest of this post, which started with a long winded (and superfluous) introduction, aims to address this question and to do so in the spirit of the complex systems department. 

## Number types and binary arithmatic

Up until the early 20th century, "A Computer" was a person doing the work of calculating things. Today of course, when we say this word we mean an electronic device which performs computations using something that is stored on it **physically**. This information, a list of symbols representing ones and zeros which are not so dissimilar to lines cut into a clay tablet, can be implemented in many ways. For example, they can be on a flexible plastic tape that is coated by a thin layer of iron. Segments of this layer can then be magnetized, with the magnetic North pointing one way or the other. Manipulating these stored bits of information, which is done by flipping the direction of the magnetic poles, is the way we change what is stored on the tape.

## Counting with cogs

Our brains are pretty great at remembering every lyric of an album we listened to as a teenager. But  we're not so good at keeping track of too many digits, especially when out of context. Some people can recall Pi to fantastic precision but that's a feat achieved by specific and targeted practice. A typical person only has a working memory of about 7 digits, but for me personally I feel it's much less. This is at least part of the reason children use fingers when they first learn basic arithmatic. Keeping track of the digits, with their digits (which is why we call them digits to begin with!).
So external memory is essential when performing arithmatic, but to apply a mechanical process to do mathematics for us we need to add some logic. You can do that in as many ways as your imagination allows for, but a nice way is to use machines. 

I've recently visited the Musée des Arts et Métiers (Museum of Arts and Crafts) in Paris. In typical Parisian fashion, it's this wonderful historic building. The spacious and bright exhibition halls include a converted gothic church which, as designed, evoke a gasp as you enter. In lieu of a pulpit and religious iconography, a full size Vulcain rocket engine (of the type used on the Ariane 5 vehicle), as big as a small bus and at least as prepared to leave this earth as the biblical angel it replaced. 

The museum holds all manor of historic scientific instruments, and is well recommended, but one relic I was particularly interested in is the Pascaline, Pascal's mechanical calculator. 

![img.png](pascalino.png)

This machine has a panel with several dials, each inscribed with the digits 0-9. Just like when writing a number on a piece of paper, each dial position encodes a different decade (units, tens, hundreds and so on).
This enables you to input a number to the calculator, which serves as kind of a mechanical memory. Using it frees the user's mind from having to remember the first number of your computation. Having input a first number,  the dials are repeatedly turned to input the number to be added or subtracted. The display will then show the result of the computation. Addition can be repeated multiple times to achieve multiplication and by the same token subtraction can be repeated for division (with remainder). You do need to keep track of the number of times you added or subtracted manually though (as far as I could tell).

Counting with a dial is nothing fancy. If I set the dial to, say, 2 and then turn it by 2 positions it will show the value "4". I've successfully performed the computation 2+2=4. However, if the dial was at 9 and I now try to add 2, I will get "clock arithmatic": 2+9=1. This is the result modulo 10, my counting basis. This is not how multi-digit numbers are added though. We were taught, at a very young age, that we need to "carry the 1". So when we add two digits and the result is larger than 10 (or whichever number basis you may be working in) you need to pass the overflowing digit to the following position to the left.

### Half adder and full adder

A device implementing this behaviour, where the overflow digit is passed on (more commonly referred to as "carried over") is called a half-adder. A half adder takes two input numbers and outputs a sum and a carry digit. Two inputs, two outputs. This is insufficient for a functional adding machine as you also need a carry-in digit (to check if the previous position itself has overflowed). A device with this, more intricate, behaviour having **three* inputs (two numbers + carry in) and two outputs (sum and carry-out) is called a full adder. 

In the Pascaline, under the hood, a set of full adders is implemented with cogs. Each cog meshes with the ones before and after it such that a full rotation of the previous cog drags the next one by one position. Doing this smoothly and is the heart of the innovation introduced here. 

## Calculating with qubits

Surely, all the remains to be explained is how to make a quantum full-adder. One which can be implemented with atoms, photons or trapped ions. If that's the case, there's hardly nothing interesting left to be said. In principle, if all you want to do is a single computation which does not feed into a next algorithmic step, that may be indeed true. That would be a very bad use-case for a quantum computer though. Unless your arithmatic step is the very last one in the quantum circuit, it surely feeds into other algorithmic building blocks.

## Qubit architectures

Quantum computers are not that special. Though the magic sauce that power their computation cannot be though of as a more elaborate set of mechanical cogs, the computer architect still needs to decide on a method to encode information on their device. 

Like the encoding of digits as positions on a set of cogs, or as on the orientation of magnetic segments on a piece of tape, 

## Automation of number encoding



## A quantum mechanical full adder

## Uncomputation







