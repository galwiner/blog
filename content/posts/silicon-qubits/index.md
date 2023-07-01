---
title: "The basics of quantum-dot qubits"
date: 2023-06-08
math: true
featured_image: 
draft: false
---

[//]: # (This post follows, to some extent, the following papers:)

[//]: # (* [Marco De Michielis et al 2023 J. Phys. D: Appl. Phys. 56 363001]&#40;https://iopscience.iop.org/article/10.1088/1361-6463/acd8c7&#41;)

[//]: # (* [Maurand, R., Jehl, X., Kotekar-Patil, D. et al. A CMOS silicon spin qubit. Nat Commun 7, 13575 &#40;2016&#41;]&#40;https://doi.org/10.1038/ncomms13575&#41;)

## So many qubits, so little time 

At the time of writing this there are at least five competing technologies vying for the quantum computing throne. Superconducting qubits, trapped ions, neutral atom arrays and silicon (CMOS) qubits are the top contenders. The various so-called "color centers", a prominent example of which are Nitrogen-Vacancy centers in diamonds, arguably lagging behind. 

 Using an eye-rollingly terrible expression, it's 'The Zoo of Qubits'. They only reason I am willing to use it is because I do believe some of them are cute but useless, some are scary and may bite you, and It's very likely most of them will be extinct soon.

Here I will give a brief introduction to what quantum dot qubits are. Although not currently as advanced as trapped ions and superconducting qubits, the supposed incumbent and leading contender for the qubit crown, they may well overtake them as they stand on the most solid of foundations. 

## Solid state physics

To start speaking about quantum dots, we must choose our own adventure. We can go left, deciding we don't care (or we already know) about how to control the motion and orientation of electrons in semiconductors. That's a much shorter path, but you will miss out on some intricate but fun physics. On the other hand we may decide to dive into the entirety of solid state physics, starting at page 1 of [Ashcroft & Mermin](https://www.amazon.com/Solid-State-Physics-Neil-Ashcroft/dp/0030839939) and spiralling to a 5-piece series that will be read by no-one and will benefit even fewer people. Here, I'll try to guide us through the long route, but doing it like an elite tour-de-france cyclist on a magnificent alpine road (Yeah, I've been watching the Netflix show the Tour this weekend). Meaning: we'll do it as quickly as possible and not pause to take in the views. Just the bare-minimum, as I perceive it. 

## The flow of electrons in a periodic forest

### The classical picture

In quantum mechanics we think of particles as waves sometimes. A wave, unlike a traditional billiard-ball particle, doesn't have **a** position. Is the wave here? Is it there? It's an _extended_ entity. It has different wave amplitudes at different positions and if you look at more than one wave arriving at the same position, they act as the sum of their individual amplitudes. Quantum mechanics tells us the probability of finding the particle (or particles) at a specific location is given by that amplitude squared. All this is fairly straight forward. 

Our goal here is to get some intuition for how electricity flows through a medium. There's a simple but totally useful model for this called The Drude model after the 19th century German physicist Karl Drude. Here's the model: electricity is the flow of electronics which we imagine to be billiard balls hopping about in a forest of immovable pillars.

![drude model picture (taken from wikipedia)](drude.png)

The electrons are the blue circles, and they are moving between large and red circles which are immovable. The electrons are mobile, and are of course electrically charged. This means that if we apply a uniform electrical field (for example by connecting a battery) then they will start moving. In the image above, this is marked as their "drift velocity" \\(v_d\\) which points to the right. Because the electrons have _negative_ charge you can see the electric field direction \\(E\\) and the electrical current \\(I\\) are actually pointing against the direction of the drift velocity. This is what we now call "conventional current" which was chosen such that the direction of the current is out of the battery positive terminal and into the battery negative terminal. In retrospect, it's backwards, but we're stuck with it. 


What we get is moving particles that have some probability of colliding with an immovable ion and bouncing off of it, elastically. When they do, their velocity changes, but as they are continuously acted on by the force due to the electrical field they will eventually keep on trucking towards the positive battery terminal. Because this process has a stochastic (random) nature to it, it becomes most comfortable to start asking questions about things like the average momentum of the electrons.  

This is a very simple idea, but it already contains a fair bit. For example, we can very easily extract Ohm's law from here. See the [wiki article](https://en.wikipedia.org/wiki/Drude_model) if you're interested. But a lot more can be done with this simple starting point which is always the hallmark of a nice scientific theory. However, we can experimentally show this cannot be the complete description of how electrons behave. 

### Why do we need quantum electrons?

How do we know the Drude model can't be the whole story? Well one good clue is that when you cool some metals down their resistance to the flow of electricity becomes exactly **Zero**. Not close to zero, not "very small", it becomes non-existent. How can that be, if we are thinking of Drude's immovable ions? Well, it can't be. And yet, it is. And we've known about this behavior for a fairly long time. The ability to cool thing to cryogenic (anything below 0 °C is technically cryogenic) temperatures dates back to the 19th century. Oxygen, which becomes liquid at -180 °C, was first liquefied in 1877. Liquefaction of Helium, which occurs at -269 °C, was first acheived in 1908 by the Dutch physicist Heike Kamerlingh Onnes. He then used this ability to measure the electrical resistivity of metals at cold temperatures and discovered that the resistivity of Mercury (the element Hg) disappears when placed in a bath of liquid helium. 

There are other bits of experimental evidence that tell you the Drude model is incomplete. With modern eyes it shouldn't be surprising, for example, that to give a more complete description of electronic motion we would like to include the wave nature of the electron, the fact is has spin as well as charge and maybe bring some more complications into the model to account for things like vibrations of the ions (those red circles from before). 

### What is a Solid? 

To move forward we need to pause for a moment and think about something quite basic. What's a solid? What makes the "solid" in "solid state physics"? You can think of the high school definitions of a solid. It "opposes shear forces" for example. That means that if you can glue it to the ground and push it from the side, and it will try to push back then *it* is a solid. Water can't do that. Neither can Jell-O. That's a fair description, but what happens microscopically? How does a solid look like at the atomic scale. 

The way we learn to think about solids as physicists in our undergraduate training is as _Crystals_ 💎. A crystal in this context means a periodic tiling of atoms. Crystallography is really rich and interesting and there's tons to be said about it from physics-theoretical, mathematical and experimental points of view. It is a large body of work that has been explored for over a century but is ever evolving. In the 1980s it was considered as a fundamental truth that crystals must be absolutely periodic and obey certain rotational symmetry properties. It was then shown by experimentalist Dan Shechtman of the Technion in Israel that was not the case, a discovery for which he was awarded the P Nobel Prize in Chemistry in 2011. Today, because we are able to control single atoms, and single atomic layers, we have access to man made crystal structures. An example of this is the one-atomic-layer thick sheet of carbon atoms called Graphene. This material and its properties have been actively explored since the 1990s, produced Nobel Prize laureates. More recently, the ability to stack such monolayers and even to change the angle between them as they are stacked (so-called "magic angle graphene") has produced even more rich physics (see for example pioneering work by [Pablo Jarillo-Herrero at MIT](https://physics.mit.edu/faculty/pablo-jarillo-herrero/)).

### Electron waves in solids

It's not billiard balls and immovable barriers then. But what is it? It's waves of course. Waves of electrons flowing through a periodic set of barriers. When electrons, described as waves, flow through periodic obstructions something magical happens. Bloch electrons. 








# I come from a land down under

# CMOS 

# Two Dimensional Electron Gas

# The Pauli Blockade

# Reading out qubits 

# 