---
title: "The basics of quantum-dot qubits (2): the makings of a silicon qubit"
date: 2023-08-14
math: true
draft: true
---

# Previously on : Silicon Qubits

[Last time]({{<relref path="../silicon-qubits/index.md" >}}) we covered some basic concepts in solid state physics. It wasn't everything there is to know, and does not make us experts on silicon qubits, but it's a fair bit. It's hopefully enough to get us through this post where we go deeper into this technology. 

I'm personally not an expert. There was a time in my professional past when I worked on electrical transport in magnetic systems. That's when you make current flow through little pieces of magnetic (and non-magnetic) materials and explore what happens. Though quantum effects were involved, it was not the sort of single quanta physics that occurs in silicon qubits. The currents involved in my research were relatively macroscopic - measured in milli- or \\( \mu \\)- Amperes. When manipulating quantum information we work with single electrons, which is orders-of-magnitude more subtle. 

Why should you trust me then? Why is my take on silicon qubits remotely credible? Well, you shouldn't! I don't trust me either. Luckily, to cover this material with a modicum of the respect it deserves, I was able to recruit a surprise guest. 

# Run Will, Run! 🏃

A couple of years ago I was working for a company doing quantum control. I did a fair bit of work with clients, it was one of the best parts of the job as I got to work with a bunch of different qubit technologies. But I did have a favorite gang of clients to work with. It was (and is) a lovely collection of people working on the opposite side of the globe.

This is well known in the quantum community, but if you're an outsider or newcomer you may not be in on it. Australia is a quantum powerhouse. It's not just Rugby, Surfing and Kangaroos (though those would already be enough!), they're top physicists too. I got to work with some of the premier Australian scholars developing silicon qubits: the labs of Prof. Andrew Dzurak and Prof. Andrea Morello. Both teams hail from UNSW, the University of New South Wales, which is located in Sydney.

When we started working together it was during the height of Covid-19. Although I really wanted to physically be in Sydney to help set up the experiment they were working on, I could not. Australia was hermetically sealed, as was most of the world. So I had to help remotely, bridging both a time-difference and the face you aren't in the lab and can't fully see what's happening as things are being set up. It felt like being a technician helping to install an air-conditioner by shouting suggestions through a key-hole. In any case, it went well and I kept in touch with some of the students working there. 

In particular, Will Gilbert from the Dzurak group was one of the first people I worked with. Will is one of the nicest physicist you'll ever meet though he insists on calling himself an engineer. He also an ultra-marathon runner so i don't know if his judgment can be trusted. Well had since completed his Ph.D. and is now one of the first employees in a company working on making million-qubit devices built with the technology he helped develop as a student. 

I've been writing my usual rambling prose in the first person up until this point. Everything below is a collaboration with Will and should make a lot more technical sense! If it doesn't it's his fault. 

# Fab-rication. It's fabulous.

The technology which allows us to create small structures in Silicon is arguably one of the most advanced abilities humans have ever developed (but it's going to be a simple argument to win). Today we are able to shape Silicon (and other materials) to form complex structures made of shapes that are measured in single nanometers. 

This would have already been impressive if the way to do this was to shoot single atoms or single electrons at a material, and chew away at it very slowly, ending up with the shape we wanted. The size of the atom is below a nanometer and so that would have been a process similar to an ant forming a sculpture from a sugar cube by thoughtfully removing individual sugar grains. This method, as you can imagine, is painstakingly slow. If we wear our capitalists' visor for a moment, the consequence would have been impossibly expensive computer chips and the computer revolution would never have happened. Nevertheless, there do exist such methods to shape silicon like electron-beam or ion-beam lithography and in some cases they're very useful.  

The way we make these small structures in industry is devilishly clever and incredibly quick. We use light in a process called Photo-lithography (almost literally "writing with light"). This is a process where the shapes we want to make in Silicon are projected (with light) onto the material. Using various chemical processes it's possible to eat away ("etch") areas where the light touches and leave others untouched (or vice-versa). Instead of the ant chipping away at the sugar cube, bit-by-bit, it's more like developing a picture. 

Photo-lithography is an amazing process. Its current capabilities (in terms of resolution and throughput) seem to defy the laws of physics and single state-of-the-art lithography machines can cost hundreds of millions of dollars. We can go into much more depth, but we won't here. 

The point is we can pattern silicon with small structures. We can make pillars whose diameter is measured at micrometers, or less. And with that capability in our tool-belt we can enter the realm of mesoscopic physics. The physics that is small enough to feel quantum effects, but large enough for classical physics to be applicable. It's a quantum goldilocks zone.  

# Islands in the stream or: detecting the presence of a single electron 

Usually when you let electricity flow through metal, or a semiconductor, the number of electrons involved is unfathomable. An example of what happens when you go mesoscopic is that the interactions between electrons become important. After all we tend to imagine electrons as little balls with an electrical charge, and we know equal charges repel.  

If we have a sing
$$ C = \frac{e^2}{\Delta E} $$

Coulomb blockade

# What is CMOS

Complementary Metal–Oxide–Semiconductor (CMOS)

Silicon MOS (Metal Oxide Semiconductor) is a stack of silicon substrate, oxide (often SiO2, but sometimes Al2O3 or HfO), then metal gates (Al, Pd, Polysilicon, Cobalt, some common ones).

People like Morello, Rogge use the same stack but with dopant atoms (P, Sb, As, Er to name a few). These are not pure quantum dots though.

Another common one is Si/SiGe, which is a hetero-structure like GaAs/AlGaAs, usually enhancement mode though. These days much more popular since it has workable coherence times



# Controlling electrons in a 2-DEG

# Flippin' electrons. How microwave is used to set the direction of electronic spin 

Electron Spin Resonance (ESR)

# Singlets and triplets : an interlude back to basics

# A CMOS qubit



# How many qubits do you want? Silicon can make them...

Perhaps a less greedy question is:- How many qubits do we need? Ask the starry eyed quantum processor engineer and they might give you an optimistic answer something like "We think we can start to do useful things with 100,000, maybe even only 10's of thousands of qubits." Ask the grumbly quantum error correction guy, and they're more likely to tell you "Make me ten million qubits, then we talk".

The truth is, all of those numbers are a lot, and deep into the realms of scaled systems.

Qubits in silicon are tiny. If you put a million of them right next to each other in a grid, most people would still need a microscope to see the spec they created. But that's not the important part... 

The tricky thing with qubits is how to control them. They're needy little rebels, they each need their own tailored control signals. So for a million qubits, that's at least a million control signals. The control signals need wires to travel down, and they all need to be delivered to a chip sitting in the core of a bespoke refrigerator at only a fraction of a degree above absolute zero. The problem is oddly reminiscent of Mr Burns asking Smithers to get into the Spruce Moose. 

Thankfully there is one way we've managed to create systems with that many signals generated and routed around in one place.... You guessed it, CMOS.

# Electronics

# Thermal load

# Summary

