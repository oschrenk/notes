# Mathematics #

## Interpolation ##

Interpolation describes a group of methods (and problems) that tries to find a continuous functiona able to map data points within the range of a given discrete set of data points in order to evualuate the function at any given point.

Given \\(n +1\\) data points \\((x_i, f(x_i))\\), \\(x_i)\\) with \\(x_i \neq x_j, i = j, i,j \in \mathbb{N}, i <= j\\) the interpolation problem is to choose the parameters \\(a_j\\) in  such a way so  that the function \\(g(x, a_0,\dots, a_n) = f_i(x)\\) for all \\(x_i\\).

Or concentrating on linear interpolation problems in which \\(g(x)\\) is only linearly coupled to \\(a_j\\)

\\[g(x, a_0,\dots, a_n) = a_0 + a_1g_1(x) + a_2g_2(x) + \dots + a_ng_n(x)\\]

### Linear interpolation ###

The most basic and one of the most used approach developed by Isaac Newton is the linear interpolation which, interpreted geometrically, draws a straight line between two data points \\((x_0, f(x_0)), ((x_1, f(x_1))\\).

\\[g(x) = f(x_i) + (x-x_0)\frac{f(x_{i+1})-f(x_{i})}{x_{i+1}-x_{i}}\\]

### Polynomial interpolation ###

While the linear interpolation is quick to compute it has its drawbacks like having an error proportional to the square of the distance of the data points and not being differentiable at\\(x_i\\).

As a more generalized approach one can try to find a higher degree polynom \\(p\\) that solves all the equations \\(p(x_i)=f_i\\) in a vector space with degree \\(n\\).

While being harder to compute ( \\(\mathcal{O(n^2)}\\)), the polynomial interpolation offers the advantage at being differentiable at \\(x_k\\) and a lower error. Finding the equation is reduced to finding a solution to a system linear equations

### Piecewise Interpolation ###

Because polynoms of higher degrees tend to oscillate between equidistant points it is normal practive to interpolate piecewise. This approach relies on the the differentiality on \\(x_i\\).

#### Spline Interpolation ####

Piecewise Interpolation of the second or third degree is normally called Spline Interpolation, named so after the splines used for ship or airplane construction by Schoenberg.

\\[g_i(x) = a_j + b_j \cdot (x - x_i) + c_i \cdot (x-x_i)^2 + d_i \cdot (x-x_i)^3\\]

for \\(x_{i-1}\leq x\leq x_i\\)

In order to solve the system of linear equations we need \\(4n\\) constraints. \\(2n\\) are given because \\(g(x_{i-1}) = f(x_{i-1})\\) and \\(g(x_{i}) = f(x_{i})\\) for \\(i = 1 \dots n \\)

Because of their construction cubic splines have to be differentiable two times at their knots \\(x_i\\), giving us \\(2n -2\\), one for each segment between two knots, more constraints because \\(g'(x_{i}) = g'(x_{i+1})\\) and \\(g''(x_{i}) = g''(x_{i+1})\\) for \\(i = 1 \dots n \\).

The last two boundary conditons can be chose depending on the use case, for natural splines \\(g''(x_{0}) = 0\\) and \\(g''(x_{n}) = 0\\) is used.

## Eulers Identity ##

\\[ e^{i \pi} + 1 = 0 \\]

What does this mean. Let's start with \\(\pi\\). Everybody likes \\(\pi\\).

\\(\pi\\) is the ratio of circle diameters to its circumference

Fun fact. The height of an elephant (from foot to shoulder) \\(h = 2 \pi d\\) (\\(d\\) being the diameter of its foot).

Degrees are an arbitrary number, even aliens would use radians

\\[180° = pi\\]
\\[360° = 2pi\\]

\\(e^x\\) is its own differential

\\[(e^x)' = e^x\\]

It's also

\\[e^x = x^0/!0 + x^1/!1 + x^2/!2 + x^3/!3 + x^4/!4 + ...\\]

And you probably know it better as

\\[e = 2.7 1828 1828 45 90 45 ...\\]

But

\\[sin x = x^1/!1 - x^3/!3 + x^5/!5 - x^7/!7 +  ... \\]
\\[cos x = x^0/!0 - x^2/!2 + x^4/!4 - x^6/!6 +  ... \\]

Now it get's freaky. Complex Numbers.

\\[i^2 = -1\\]

Complex numbers add another dimension and can therefore be written as distance to 0 point \\(4 +3i = 5(\cos \theta + i \sin \theta)\\)

\\[ e^{i \pi} + 1 = 0 \\]

[Oliver Humpage at Ignite Bristol, A beautiful equation](http://www.youtube.com/watch?v=vHaPyuEkK4A&feature=player_embedded)
