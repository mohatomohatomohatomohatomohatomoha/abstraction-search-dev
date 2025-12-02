# abstraction-search-dev

Abstraction Search is a search algorithm created to address and combat the curse of dimensionality. 

The first instruction is to abstract segments of the environment into their own scalar fields. The simplest way to do this is to imagine the environment as a sum of $n$-dimensional superellipses. The equation for simulating these scalar fields, which will be referred to as \textbf{objects}, will be discussed later. Let $\mathcal{S}(x_1,\dots,x_n)$ be the sum of all objects for an $n$-dimensional scalar field $\mathcal{S}$. 

The second instruction is to calculate the Lagrangian of the line integral of $\mathcal{S}$ from $(p_1,\dots,p_n)$ to $(q_1,\dots,q_n)$. This can be given by the expression
$L(x_1(t),\dots)=\mathcal{S}(x_1(t),\dots)\sqrt{\dot{x}_1^2+\dots+\dot{x}_n^2}$
where $x_i(0)=p_i$ and $x_i(1)=q_i$.

The third instruction is to create a Euler-Lagrange equation for each dimension to solve for the minimal path $x_i(t)$ for $i\in\{\mathbb{Z}\cap[1,n]\}$. This creates $n$ differential equations. The equation for a given $i$ is
$\frac{d}{dt}(\frac{dL}{d\dot{x}_i})=\frac{dL}{dx_i}.$
Symbolic differentiation libraries are utilized to find the symbolic expression of each side of the equation.

The fourth instruction is to replace every instance of a higher-order derivative of $x_i$ with the alternative given by Finite Difference Method, creating $n \cdot k$ system of equations, where $k$ is the number of discretized grid points for each path. $k$ is dependent on the maximum path length in a way that
$k=\max(|p_i-q_i|) \cdot \omega$
where $\omega$ is the number of grid points per unit and $\omega\in\mathbb{Z^+}$. The fact that k is solely dependent on the distance between two given dots relative to a single axis is crucial to maintaining polynomial runtime.

The fifth instruction is to choose an initial guess for the optimal path. Iteration of the Newton-Raphson method repeatedly applies a re-correction vector to the path until the path approaches a critical point of the line integral functional, whether it be a local minimum, maximum, or a saddle point. Let $\delta$ be the number of times the Newton-Raphson method is iterated. For $\delta$ to be acceptable, the optimal path or a near-optimal path must be found in a reasonable amount of time. Tuning $\delta$ is necessary depending on the complexity of the environment or the accuracy needed.

At this point in the algorithm, a near-optimal/optimal path is found with some confidence. The confidence function can be estimated via
$\mathcal{C}(\mathcal{S}(x_1,\dots,x_n)) \approx \frac{\mathcal{A}(\delta,\beta)}{2^{(\beta-\xi)} -(\beta-\xi)-1}$
where $\beta$ is the number of objects, $\xi$ is the number of overlapping objects, and $\mathcal{A(\delta, \beta)}$ is the average number of objects discovered from Newton-Raphson method out of $\beta$ total objects after $\delta$ iterations. This confidence score is not guaranteed to work for all possible values of $\mathcal{S}$, however it is a decent heuristic for estimating the chances that the optimal path is found. The estimated bounds of $\mathcal{C}$ is $\mathcal{C}\in[0,1)$ for $\beta\in\mathbb{Z^+}$ and $\delta\ge0,\delta\in\mathbb{Z}$, giving that $\mathcal{A}(\delta,\beta)<2^{(\beta-\xi)} -(\beta-\xi)-1$




concepts to use in code:
- lagrangian
- euler-lagrange equation
- functional minimization/maximization
- n-dimensional contour plots
- line integrals
- parameterization
- finite difference method
- newton-raphson method
- jacobian matrix (possible help for path recorrection? dont know exactly what this is but this seems useful for boundary value problems like the one given by FDM https://pages.hmc.edu/ruye/MachineLearning/lectures/ch2/node7.html)
