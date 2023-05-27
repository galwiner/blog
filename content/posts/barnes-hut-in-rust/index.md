---
title: "Barnes Hut in Rust"
date: 2023-05-27
math: true
draft: false
---

# Apprenticeship 

The education system in the UK has an apprenticeship path built into it.  This path allows young people who wish to finish the purely academic chapter of their studies at 16 to acquire vocational skills.  I've never done an apprenticeship myself, but I was enamored by the idea when I heard about it as a schoolboy. 

How I imagine an apprenticeship, which may be very different from what a British apprenticeship actually looks like, is like a movie montage.  The young padawan becomes an expert via a progression of intricately designed and precisely exacting exercises.  It's the fantasy of having an all-knowing responsible adult thoughtfully guiding you.  A very comforting dream which for me was also fuelled by books such as
[Shop craft as soul craft](https://www.amazon.com/Shop-Class-Soulcraft-Inquiry-Value-ebook/dp/B00273BHPU) (which I read slightly later in life) and 
[Zen and the art of motorcycle maintenance](https://www.amazon.com/Zen-Art-Motorcycle-Maintenance-Inquiry/dp/0060589469) which I really loved as a high school-er. 

As I learned through subsequent life experience, there is seldom a responsible adult.  You need to fumble about (hence the name of this blog) and construct a coherent set of experiences that adds up.  Nevertheless, "Zen & the Art" gives at least one piece of practical advice on how one should approach the journey:

>Mountains should be climbed with as little effort as possible and without desire.  The reality of your own nature should determine the speed.  If you become restless, speed up.  If you become winded, slow down.  You climb the mountain in an equilibrium between restlessness and exhaustion.  Then, when you're no longer thinking ahead, each footstep isn't just a means to an end but a unique event in itself.  This leaf has jagged edges.  This rock looks loose.  From this place the snow is less visible, even though closer.  These are things you should notice anyway.  To live only for some future goal is shallow.  It's the sides of the mountain which sustain life, not the top.  Here's where things grow. 
>
> But of course, without the top you can't have any sides.  It's the top that defines the sides.  So on we go—we have a long way—no hurry—just one step after the next—with a little Chautauqua for entertainment -- .Mental reflection is so much more interesting than TV it's a shame more people don't switch over to it.  They probably think what they hear is unimportant but it never is.

## A good project and good guides

I've had a long-standing project with multiple false starts over the years.  Actually, I have a proverbial drawer full of them, but in this one, the goal is to simulate the motion of many interacting particles on a computer screen.  It's natural for any physics student to consider doing this, as it has been a reference setup since the dawn of the discipline: the ideal gas, properties of solid matter, and the structure of galaxies. 

While reading about simulating the gravitational interaction of many bodies, one algorithm caught my eye and imagination.  It's not necessarily because it is particularly elegant or ingenious (though it ticks both boxes).  Its output looks pretty, even if you don't understand the details.

That, I think, is an essential factor when selecting a project.  It should be sufficiently simple (and this one seemed to be simple because it was one of the first algorithms in the book), and it should be exciting (to you).  This particular one had the added feature of being very visual, which is my preferred mode of thought. 

The omniscient adult being in absentia, is a curious protagonist doomed to hack about like a drunkard?  That may well be the case.  But if you can find someone, preferably someone slightly more experienced, to walk with you, that's significantly better.  Some people enjoy walking alone.  I like company.

So on this expedition, I recruited not one but two much more experienced guides: Joe Lee Moyet and Matan Cohen.  Each of these gentlemen has orders of magnitude more programming experience than I will ever have.  I could say more kind words about Joe and Matan, but I'll avoid this one tangent for now.  Anyway, thanks, Joe and Matan!  And for the rest of us, I recommend finding a good guide to help you up the mountain with as little effort as possible. 

## The computational complexity of many particles moving under gravity

Our goal today, then, is to draw circles on a computer screen that gravitationally act upon each other. For that, let's quickly recall some principles of mechanics and how one can find the equations of motion of interacting objects. 

To compute the gravitational attraction of two particles you apply Newton's law of gravitation. 

$$ m\ddot{\vec{r}}_1 = \frac{G m_1 m_2}{|\vec{r}_1-\vec{r}_2|^3}(\vec{r}_1-\vec{r}_2)$$
$$ m\ddot{\vec{r}}_2 = \frac{G m_1 m_2}{|\vec{r}_2-\vec{r}_1|^3}(\vec{r}_2-\vec{r}_1)$$

This is a nice set of equations as you can see both Newton's second law and his third (action and reaction) participating. 

If we only have two objects interacting, there is a close form solution. Observationally, the solution was formulated by Kepler in his laws of orbital motion. Newton was then able to find equations describing the motion from the basic principles he had laid down. 

The thing about gravitational attraction is that the range of the force is infinite. We see this from the equations above: the force only disappears when the distance \\(|\vec{r}_1-\vec{r}_2|\rightarrow \infty\\). 

So if you want to calculate the force acting on each particle in a collection, you really have to take into account each individual particle. How many computations is that? About \\(n^2\\). 


[//]: # (When I first heard about Python it was sometime around winter of 2008.  I had a student job testing software and all the seasoned programmers there were quite happy about doing a first or second big project in the language.  I was a physics undergraduate and had barely done a basic programming course, taught in C. )



[//]: # ()
[//]: # (## The Rust programming language)

[//]: # ()

[//]: # ()
[//]: # (## The Quad Tree - a data structure for efficiently partitioning spatial detail)

[//]: # ()
[//]: # (## The Barnes-Hut algorithm)

## WASM - Web Assembly and a live example



