---
title: "Barnes Hut in Rust"
date: 2023-06-01
math: true
featured_image: "header.png"
draft: false
---

# Apprenticeship 

The education system in the UK has an apprenticeship path built into it. This path allows young people who wish to finish the purely academic chapter of their studies at 16 to acquire vocational skills. I've never done an apprenticeship myself, but I was enamored by the idea when I heard about it as a schoolboy. 

How I imagine an apprenticeship, which may be very different from what a British apprenticeship actually looks like, is like a movie montage. The young padawan becomes an expert via a progression of intricately designed and precisely exacting exercises. It's the fantasy of having an all-knowing responsible adult thoughtfully guiding you. A very comforting dream which for me was also fuelled by books such as
[Shop craft as soul craft](https://www.amazon.com/Shop-Class-Soulcraft-Inquiry-Value-ebook/dp/B00273BHPU) (which I read slightly later in life) and 
[Zen and the art of motorcycle maintenance](https://www.amazon.com/Zen-Art-Motorcycle-Maintenance-Inquiry/dp/0060589469) which I really loved as a high school-er. 

As I learned through subsequent life experience, there is seldom a responsible adult. You need to fumble about (hence the name of this blog) and construct a coherent set of experiences that adds up. Nevertheless, "Zen & the Art" gives at least one piece of practical advice on how one should approach the journey:

>Mountains should be climbed with as little effort as possible and without desire. The reality of your own nature should determine the speed. If you become restless, speed up. If you become winded, slow down. You climb the mountain in an equilibrium between restlessness and exhaustion. Then, when you're no longer thinking ahead, each footstep isn't just a means to an end but a unique event in itself. This leaf has jagged edges. This rock looks loose. From this place the snow is less visible, even though closer. These are things you should notice anyway. To live only for some future goal is shallow. It's the sides of the mountain which sustain life, not the top. Here's where things grow. 
>
> But of course, without the top you can't have any sides. It's the top that defines the sides. So on we go—we have a long way—no hurry—just one step after the next—with a little Chautauqua for entertainment -- .Mental reflection is so much more interesting than TV it's a shame more people don't switch over to it. They probably think what they hear is unimportant but it never is.

## A good project and good guides

I've had a long-standing project with multiple false starts over the years. Actually, I have a proverbial drawer full of them, but in this one, the goal is to simulate the motion of many interacting particles on a computer screen. It's natural for any physics student to consider doing this, as it has been a reference setup (at least in thought) since the early days of the discipline: the ideal gas, properties of solid matter, and the structure of galaxies. 

While reading about simulating the gravitational interaction of many bodies, one algorithm caught my eye and imagination. It's not necessarily because it is particularly elegant or ingenious (though it ticks both boxes). Its output looks pretty, even if you don't understand the details.

That, I think, is an essential factor when selecting a project. It should be sufficiently simple (and this one seemed to be simple because it was one of the first algorithms in the book), and it should be exciting (to you). This particular one had the added feature of being very visual, which is my preferred mode of thought. 

And so, with the omniscient adult in absentia, is a curious protagonist doomed to hack about like a drunkard? That may well be the case. But if you can find someone, preferably someone slightly more experienced, to walk with you, that's significantly better. Some people enjoy walking alone. I like company.

For this expedition, I recruited not one, but two, much more experienced guides: Joe Lee Moyet and [Matan Cohen(@matanco64)](https://github.com/matanco64). Each of these gentlemen has orders of magnitude more programming experience than I will ever have. I could say more kind words about Joe and Matan, but I'll avoid this one tangent for now. Anyway, thanks, Joe and Matan! And for the rest of us, I recommend finding a good guide to help you up the mountain with as little effort as possible.

There are several take-home messages for me here, which made this project reach completion this time. It wasn't too hard. I was excited about the topic. it fit well with what I feel is fun doing. I piggybacked another skill I wanted to learn (the Rust programming language) on top of something I was already excited about to get something of a two-for-one, and most importantly I had guides I wasn't afraid to look stupid next to and who were at least as excited about the project as I was.    

## The computational complexity of many particles moving under gravity

Our goal today then is to draw circles on a computer screen that appear to be gravitationally action upon each other. For that, let's quickly recall some principles of mechanics and how one can find the equations of motion of interacting objects. 

To compute the gravitational attraction of two particles you apply Newton's law of gravitation. 

$$ m\ddot{\vec{r}}_1 = \frac{G m_1 m_2}{|\vec{r}_1-\vec{r}_2|^3}(\vec{r}_1-\vec{r}_2)$$
$$ m\ddot{\vec{r}}_2 = \frac{G m_1 m_2}{|\vec{r}_2-\vec{r}_1|^3}(\vec{r}_2-\vec{r}_1)$$

This is a nice set of equations as you can see both Newton's second law and his third (action and reaction) participating. 

If we only have two objects interacting, there is a close form solution. Observationally, the solution was formulated by Kepler in his [laws of planetary motion](https://en.wikipedia.org/wiki/Kepler%27s_laws_of_planetary_motion). Newton was then able to find equations describing the motion from the basic principles he had laid down. 

If you have three bodies, [famously](https://en.wikipedia.org/wiki/Three-body_problem), there's already no closed form solution. It's the entire premise of a series of sci-fi novels, the [first of which](https://www.amazon.com/Three-Body-Problem-Cixin-Liu/dp/0765382032) I highly recommend. 

There are certainly no such solutions if you have \\(n\\) interacting bodies. This is because the system becomes chaotic, but I will not delve into the details of why that is the case here. You can [read about it](https://link.springer.com/chapter/10.1007/978-1-4684-5997-5_2) if you are interested. 

To tackle the motion of many particles we must then do so numerically. The thing about gravitational attraction is that the range of the force is infinite. We see this from the equations above: the force only disappears when the distance \\(|\vec{r}_1-\vec{r}_2|\rightarrow \infty\\). 

So if you want to calculate the force acting on each particle in a collection, you really have to take into account each individual particle. How many computations is that? About \\(n^2\\). You pick one particle out of \\(n\\) and then need to pick each of the other \\(n-1\\) particles left to make the force calculations. If you have \\(10^4\\) particles you need \\(10^8\\)) calculations. Mind you, that's the number of calculations you need to make _each time_ you update the position of your \\(n\\) bodies. And, indeed, you may want more objects on screen. Quadratic computational overhead is a dear price to pay. 

## The Quad Tree - a data structure for efficiently partitioning spatial detail

The name of the game is to find a strategy for making a computation with quadratic computational complexity to one that is cheaper. Maybe logarithmic, probably \\(O(n\cdot log(n))\\).

The first step is to efficiently store data about our particles. This can be done by taking advantage of the non-uniformity of particle distribution in our simulation space. There is often at least some structure. Namely, regions where they are more, or less, tightly packed together. 

A Quad-Tree is a two-dimensional extension of a binary tree. Instead of each node having _Left_ or _Right_ leaves, it has exactly four children representing the Northwest, Northeast, Southwest and Southeast equally subdivided quadrants of a square (the original node).  

### Divide and conquer 

When building a quad tree, we decide on some particle capacity each quadrant is allowed. If the capacity is exceeded (as we add another particle), then a subdivision of the quadrant occurs, and we distribute the particles amongst the new collection. An example is shown in the image below where we set the capacity (arbitrarily) to four particles and when there are 5 particles we must subdivide. 

![](division.png)

As the number of particles increases the number of repeated subdivisions must also increase, as in the following image taken from the [wikipedia article](https://en.wikipedia.org/wiki/Quadtree#:~:text=A%20quadtree%20is%20a%20tree,into%20four%20quadrants%20or%20regions) on this topic.

![](Point_quadtree.png)

The utility of the quad tree is two-fold. It has more detail (more nodes) where more particles are present, and it enables finding particles stored inside it in an efficient way (because it's a tree structure). For these reasons it's extensively used in computer graphics whether gravitationally attracting particles are involved or not (e.g in the old computer game [Worms](https://gamedev.stackexchange.com/questions/158906/destructible-terrain-like-worms)).

## Integration of the equations of motion

Writing down the force equations on every particle is straight forward, if computationally taxing. But even if we have infinite computational power, we need some rule for updating the velocities and positions of those particles. An update strategy for positions based on a set of difference equations, whether they are equations of Newtonian mechanics or any other differential equations, is a (numeric, discrete) integration scheme. 

The most basic way to do this is the Euler method. Updating the velocity vector of each particle based on its acceleration, and then updating the position vector based on its velocity. 

$$
\dot{\vec{r}_i}[t]=\dot{\vec{r}_i}[t-1]+\ddot{\vec{r}_i}[t-1]dt 
$$
$$
\vec{r}_i[t]=\vec{r}_i[t-1]+\dot{\vec{r}_i}[t-1]dt
$$

How bad is this technique? We can figure it out by comparing to a Taylor series expansion of the position vector to second order: 

$$
\vec{r}_T = \vec{r}[t-1]+\dot{\vec{r}}[t-1]dt+\frac{1}{2}\ddot{\vec{r}}[t-1]dt^2+O(dt^3)
$$

The difference between this expansion and the Euler integration equations is the term \\(\frac{1}{2}\ddot{\vec{r}}[t-1]\\). So we see that at each time step the error accumulates proportionally to \\(dt^2\\). After \\(n\\) steps, each of size \\(\propto{1/dt}\\) the error is proportional to \\(dt\\). This isn't awesome performance.

A different scheme, which makes the error accumulation rate quadratic in \\(dt\\) is the [velocity-Verlet](https://en.wikipedia.org/wiki/Verlet_integration) algorithm, but there are others. The go-to algorithm used in such cases is the family of [Runge Kutta](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods) (RK) methods. Specifically RK-4 is the default integrator in many computational numerics tools, such as Matlab.

## The Barnes-Hut algorithm

Finally, we get to the Barnes-Hut (BH) algorithm. The data structure we want to use is a Quad-Tree and we set the maximum capacity of a node to be exactly one. So nodes can either contain no particles, or exactly one particle. Attempting to add a second particle to a node triggers a subdivision, turning it into four separate nodes. After this step the particles will be redistributed amongst the newly created nodes. 

But storing particles in an efficient data structure is insufficient if we want to speed up gravitational dynamics calculations. The crux of the BH algorithm, is an approximation. If a group of particles is far from a test particle, they can all be treated as a single particle (whose mass is the combined mass of the entire group) that is located at the group center of mass. This turns multiple force calculations to a single calculation. What is left is to define what "far-away" means in a quantitative way.

The recipe is as follows: first store all particles in a quad tree. Then select the first particle in the list and find its distance \\(d\\) from the center of mass of the root node, the node containing all particles in the simulation universe. The dimension of the side of this node is \\(s\\). Now calculate

$$
\frac{s}{d} <? \theta 
$$

where \\(\theta\\) is a constant between 0 and 1. If the left-hand-side of the equation is less than \\(\theta\\) treat all particles as acting from the center of mass and perform just a single calculation. If not, go to the next node in the tree and repeat the calculation. The nice thing here is that if \\(\theta\\)=0 the process converges back to the naive case: everything is, in some sense, close by. 

This approximation glosses over the fact there can be structure inside a node. For example, all particles can be in one side of the node, or the other. A physicist may say it's akin to a first term in a multipole expansion, but I won't go into more detail for now. 

## Why Rust?

That was it for describing this nice algorithm that takes a quadratic cost and makes it much cheaper. Now all that is left to say is why I wanted to implement this thing in Rust.

When I first heard about Python it was sometime around winter of 2008. I had a student job testing software and all the seasoned programmers there were quite happy about doing a first or second big project in the language. I was a physics undergraduate and had barely done a basic programming course, taught in C. It was a bit over my head, and experience level, at the time to see what the fuss was about. It was easy to play with python, I could see that immediately. It wasn't clear how this language was fundamentally different to other things at the time, but I was excited about putting time and effort into learning it because I guessed it's going to be worth my while. Reading about the Rust language these days I feel very much the same. 

The Rust programming language is a low-level language coming from Mozilla Research. It's associated with all manor of superlatives, such as "memory-safe" and "blazingly-fast" but I'll save writing about it's differentiating factors for another day. Regardless of any technical wizardry which makes it worth-while to write in, when reading about Rust I get the same feeling I did about Python years ago. I'm not saying the languages are similar or that they have similar use-cases. Just that I get that same feeling about learning it.

Indeed, beyond supposed prestige of knowing Rust in coming years it also ended up being technically worth it. The number of particles I was able to simulate while maintaining a useable frame rate goes well into the tens of thousands and more. Moreover, because Rust has tooling for compilation into Web-Assembly (WASM) it can generate files that run in the browser. This alone is, to me, reason to play with it and as evidence I include below the working application I teased for this entire post. Hope you enjoy!

## WASM - Web Assembly and a live example

[live demo](https://galwiner.github.io/blog/posts/barnes-hut-in-rust/barnes-hut.html)
