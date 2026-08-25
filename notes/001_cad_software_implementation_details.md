
So, if we had to implement a CAD software, like a parametric CAD software from scratch, how would we even start? What are the features that every CAD software must implement, similar to how every browser must somehow have some component that can parse HTTP?

I have some basic features that I think, must obviously be implemented. First of all, if we're talking about CAD, that is, Computer Aided Design, we need to ask what did mechanical designers, before CAD, like, what did the wright brothers sketch out such that their plan is descriptive enough to be able to replicate one for one without any misinterpretations? I highly doubt that back then, there are any formal algorithms that mechanical designers use to determine whether their sketch is "fully constraint", rather, it's probably more of an engineering approach, where you sketch a rough outline, and then go along with it until you spot something that wasn't fully specified, and then fix it, etc. Nowadays, this can be a lot more formalized because, in something like freeCAD, there exists some sort of "solver" or "prover" that can proof that a particular sketch is, for instance, a closed shape, and also, determine the degrees of freedom, a long with what part of the shape can still move. But there are so many different constraint you must implement, and somehow add that to the degrees of freedom to the sketch, I mean, we have stuff like:

- coincident constraint
- horizontal and vertical length
- angle
- equal constraint
- symmetric constraint
- horizontal and vertical constraint
- etc...

So, where do you even begin? Like, this seems like a similar problem in like, the kinematics of machines, like, four bar linkage and a bunch more mechanical problems, but like, it has to pretty powerful enough to basically describe everything you can imagine in the 3d world.

So, I think, we first start with a 2d geometry solver first.

# 2d geometric kernel

So, we first need to implement stuff like:

- vectors
- points
- lines
- and perhaps circles
- ellipse
- splines (a series of cubic Bezier curve with different constraints, like, G1, G2 constraints or something)

We also need to implement functions like:

- distances between points
- the point of intersection between two lines

Now, this will allow us to graph anything we want, similar to how you'd use a graphing software, but questions such as:

- "is this shape closed?"
- "with the constraints specified, what part of the sketch is still unconstrained?"

This is part of topology? What about the second question? I feel like this is part of the geometric kernel right?

So topology solves "is this wire closed?" by doing something like a connectivity graph. Later on, you'd probably need topology to do 3d stuff too.

As for checking for what parts of the sketch is constraint, that's done by the constraint solver. 

Somehow, constraints can turn into a list of predicates, in which you can use some stuff in propositional logic and like sat solver kinda stuff to resolve? Like, if we're doing normal graphing, when we specify a constraint, we essentially construct another equation and specify that these the two equations must hold and stuff right? I mean, for doing things like "the line $y = mx + b$" must be tangent to the circle $(x - x_0)^2 + (y - y_0)^2 = r^2$, we need to somehow, do some algebraic manipulations to arrive at the final configurations that you can have right? But you can't possibly do this for like, n different constraints right? that would be way too much machinery.

So you don't entirely turn geometry into propositional logic, rather, you try to somehow generalize the constraint equation and solve it, perhaps similar to how you can turn an nth order ODE into a system of first order differential equation, and then solve it through matrix methods? 

The graphing itself is done by some kind of 2d geometric kernel.

Ok, so let's first deal with points, and only points.

Every point in 2d let's say, has coordinate $\begin{bmatrix}x\\y\end{bmatrix}$. Now, we can add basic constraints such as:

- Horizontal constraint, by saying $y_1 = y_2$
- Vertical constraints by saying $x_1 = x_2$
- coincident constraint by saying $x_1 = x_2$ and $y_1 = y_2$
- horizontal distance constraint by saying $|x_1 - x_2| = c$
- vertical distance constraint by saying $|y_1 - y_2| = c$
- distance constraint by saying $(x_1 - x_2)^2 + (y_1 - y_2)^2 = c^2$

And perhaps that's all of the constraints that you really need.

Now, when we use the program, we have a bunch of points, and the only way to store it without losing any info is to store is like:

$$
\begin{bmatrix}
x_1 & x_2 & \cdots & x_n\\
y_1 & y_2 & \cdots & y_n
\end{bmatrix}
$$

Or, we can just flatten it and store it as a vector or whatever.

Now, when we start adding constraints, what we're doing is:

Adding each one of those equations into our "system of equations".

If these equations were linear, it would be a "simple" problem in linear algebra, but, it of course isn't. If this were a problem in linear algebra, you could define:

$$
\vec{x} = \begin{bmatrix}
x_1\\
y_1\\
x_2\\
y_2\\
\vdots\\
x_n\\
y_n\\
\end{bmatrix}
$$

And then, your problem turns into, find $\vec{x}$ given $\mathbf{A}$ such:

$$
\mathbf{A} \vec{x} = \mathbf{b}
$$

That is to say, we want to find the column space. And now, we can use the tools and analysis in linear algebra, that is, the dimensions of the null space would determine the degrees of freedom that we have. If we have too little constraint, our null space would have multiple basis, if we have exactly one solution, then we're perfectly and fully constraint, etc right?

But since our constraints aren't all linear, we'll need to somehow deal with that...

Essentially, this is a problem in numerical analysis, where you want to solve a system of non-linear equations. One method of doing so it to linearise it locally using the Jacobian, however, I don't quite understand how you'd be able to find ALL solutions, or even prove that given the set of constraints is "complete".

So in fact, we actually don't need to find multiple solutions I guess. As long as we find one solution, we're fine? Why? Let's start with a simple example:

Suppose you have two points, and you have the following constraint:

- horizontal distance constraint on $P_1$ and $P_2$
- vertical constraint on the two points as well

When you specify the horizontal constraint, there are in fact, 2 solutions, either $P_1$ is to the left, or to the right of $P_2$, or vice versa right? But, if we were to just place the point $P_1$ to the left of $P_2$, the closest solution would probably be to state that $P_1$ is $c$ distance to the right of $P_2$ right? 

It's the same as when you specify a corner rectangle, specify that the left bottom corner is coincident to the origin, and specify kinda the orientiation of the rectangle in the unconstrained state, such that you "nudge" the initial conditions to roughly the solution you want?
