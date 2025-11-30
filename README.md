# abstraction-search-dev

abstraction search is a search algorithm created to
address the curse of dimensionality. its time complexity is
polynomial, and compared to superpolynomial and implicitly
superpolynomial alternatives such as A* and RRT, abstraction search
shines in higher dimensions where A* and RRT cant.

step 1. abstract segments of the n-dimensional environment into
their own scalar fields, the most general being an "object" (n-dimensional prism/sphere hybrid, 
something like an n-dimensional superellipse) and let the full environment be a sum of objects. let
the sum be S(x_1,..., x_n)

step 2. find the lagrangian of the line integral of S, which is simply
L(x_1(t),...x_n(t)) = S(x_1(t),...x_n(t)) * sqrt((dx_1/dt)^2+...+(dx_n/dt)^2)
as the lagrangian of the action integral is the integrand

step 3. using euler-lagrange, create n differential equations.
example: \frac{d}{dt}\cdot\frac{dL}{d\dot{x}_i}= = \frac{dL}{dx_i}

then, using an automatic differentiation library, store each differential equation into the library
and substitute orders of derivatives of x_i with their alternatives using FDM (letting K be the
number of discrete grid points along the path. by giving boundaries by giving the start/end points
we get a boundary value problem, BVP for short)

step 4. there will be multiple functions x_i(t) that satisfy the euler-lagrange equation, as there
will always be a minimum and maximum. newton-raphson method must be utilized to make an initial guess
as the initial path is recorrected into a local minima/maxima/saddle point. for a higher confidence
that the global minima is reached, repeated iterations of newton-raphson is needed. keep track
of the path with the least cost.

step 5. the x and y values of the discretized path is given, therefore the near-optimal/optimal path
is found.

theoretical pros of AS:

- i hypothesize the time complexity of AS is likely O(n) or O(n^b) at worst for positive integer b. i
say this because the number of differential equations that are needed to be solved scales
linearly, and it is likely that K (# of gridpoints per diff. eq.) is constant/grows
polynomially. i do not have a formal proof for this, however testing AS in comparison with algorithms
such as A* is likely to reveal this nature (i have NOT done this yet). K is most likely dependent on
solely the greatest line distance between the start and target points (think x_2 - x_1 or y_2 - y_1)
and there is no reason for K to depend on the dimensions of the environment as the function x_i(t) is
still 1D no matter the dimensions of the environment
- its very suitable for cost minimization/pathfinding in higher dimensions (4D+)

theoretical cons of AS:
- really hard and time consuming to control/configure starting positions bcuz of the concepts AS
uses, object equation is quite complex and hard to generalize for all solids, annoying to figure
out object variables, automation is virtually necessary
- useless for 2D
- near-impossible to reasonably visualize higher dimensional scalar fields/solids (3D+)
- requires an automatic differentiation library or an implementation of it to be effective
- is not guaranteed to give the most optimal path, only a near-optimal path/most optimal path found
- requires a lot of care to set up, a single index error or miscalculation can throw off the entire
algorithm and give trashy results

most of the flaws are just the algorithm being rlly complex and hard to implement, but once set up
correctly the algorithm should be a life saver in many applications. for example, UAVs which
have 6 degrees of freedom in the air (x, y, z, roll, pitch, yaw) can use a generalized AS to plan
paths around obstacles in real time, something that is near-impossible with A* or RRT due to their
superpolynomial time complexities

the biggest flaw is the poor runtime in lower dimensions and the lack of guarantee for optimality.
with more intensive research, i believe that scientists can perfect this algorithm and make it
a standard in path planning in higher dimensions.

everything that i say here is theoretical, as i haven't started working on the code yet and
likely wont finish for a LONG time. regardless if my assumptions are correct or not or if
AS is even a valid algorithm to use, i'll have fun crying while working on this
