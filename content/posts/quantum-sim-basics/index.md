---
title: "Cat Qubits"
date: 2023-03-27T09:40:34+02:00
math: true
draft: false
featured_image: "puzzle_solving.png"
---

# Program, run, repeat 

Software development, like many creative activities, is an iterative process. You try to figure something out, have an initial thought about the correct way to go about things and give it a first go. In quantum algorithm development the same principle applies. You should start with a small model, see if it works and then work your way up in complexity. 

At the moment you can’t easily access a quantum processor with more than a few tens of qubits. Even for these smaller devices, queue times can be very long and larger systems are not freely accessible. Nothing is worse than developing your algorithm, sending it to sit in a queue for a week and then coming back to find the result is nonsense. Often you end up with a bunch of zeros just because you forgot an H gate somewhere (or your idea was just plain bad!)

So what should you do? Simulate of course! Simulating a quantum circuit can only be done for limited circuit sizes. This is, in fact, very good. If we could easily simulate any quantum circuit, quantum computers would be quite pointless and the vibrant quantum computing ecosystem wouldn’t exist! (also, I wouldn’t be paid to play with these things and that would be sad).

It’s worth reminding ourselves why simulating quantum circuits is difficult. The number of possible states scales exponentially with the number of qubits. Because we need to store the quantum state on our digital (classical) computer, we very quickly run out of memory with which to perform the simulation. 

Despite the annoying fact that only fairly small circuits can be simulated, it’s still very useful to perform small scale simulations. This makes seeing where our ideas lead much quicker and more efficient. Moreover, simulators can give you access to the entire state vector and its complex coefficients. This is information that no quantum experiment can ever fully give you and this can be very useful in debugging algorithms as they are being developed. 

# Quantum Simulators 
Quantum states are represented by complex-valued vectors in Hilbert space. A quantum state is transformed to another quantum state by a Hermitian matrix. These two statements contain a lot of what you need to know about quantum mechanics in general. This convenient mathematical formulation makes the simple way to simulate a circuit very intuitive. You store the initial state as a vector of complex numbers, represent each quantum gate as a matrix and then successively multiply the state by the matrices you encounter as you go through the circuit. Matrix multiplication is a problem which has quadratic complexity. So we have exponential complexity in space and a quadratic complexity in time and from the combination of these two, it’s pretty clear this is the type of problem that’s going to take a very long time to solve, even with  quite small input sizes. 

# State vectors and density matrices 

The brute-force approach described above works. It’s a bread and butter, as simple as you can do it, state-vector simulator. That’s not the only way though and it doesn’t even model some very important things. Noise, for example, which we know can never be avoided and is part and parcel of quantum computation, even in small circuits, is not treated here. If all matrices we use are Hermitian, we’re not modeling noise (decoherence) and that’s a pretty poor model. 

To improve things we can use open-quantum system formalism. One way is to use density matrices instead of state vectors. A density matrix is an extension of the state vector which includes both pure quantum states (vectors in Hilbert space) and statistical mixtures of such pure states. The idea is that maybe we simply don’t know if we have one superposition or another: maybe it’s 30% chance of having one bell state and 70% of having another. It turns out that using this extension makes it convenient to model various types of errors. Simulators using density matrices are a bit more sophisticated then, at least in what phenomena they can model, but they’re just as poor in terms of performance. 

# Clever tricks

There are many more ways to simulate quantum systems. Much more than can fit in a single post. It is instructive however to include one more simulator type because it gives us a small insight into how we can be clever and improve our computational performance, if only slightly. 

One clever trick simulators can use, at least in SOME cases, is to take advantage of situations where the circuit is not highly entangled. Of course, without entanglement there is no added computational power in a quantum model. But it’s not always the case that all qubits are entangled with all other qubits. One such situation is when the circuit simulates interacting chains of particles where there’s only interaction between near neighbors, or some subset of the particles.

In such cases, when there is limited entanglement, the state can be broken into subsets of entangled states. It’s then possible to consider much smaller matrices than the full 2^n X 2^n ones needed in the basic state vector simulation. It can be shown that for the correct conditions of limited entanglement (what this means has to be explained precisely, but probably not in this post) the simulation can be performed with a cost that is less than exponential in terms of the number of qubits. This approach is known as a Matrix Product State (MPS) simulator. 

Unsurprisingly, if we know things about the expected properties of the quantum states we are working with, or limit the kinds of operations that we are performing, we can simulate larger quantum circuits than of we just brute force it all. 

Classiq simulations and the facts of life

On the Classiq platform, simulators are on an equal footing to real quantum hardware. You can build your algorithm and select a simulator to run on on the hardware selection screen. This allows you to quickly get insights into how the quantum algorithm behaves and if things make sense. We provide simulators from a range of vendors, both on the cloud and as part of the platform itself. Running on IBM, IonQ, Rigetti, Azure and AWS requires a user to provide credentials. However, users can simulate on Nvidia and Qiskit Aer simulators without providing any credentials and completely free of charge.

Once you are happy with the simulated results you can quickly and seamlessly switch over to real quantum hardware. Quickly and seamlessly apart from the long queue times that is. But that’s just something we will have to live with until we get more cloud based quantum computers. 
