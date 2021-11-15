<!--
Hello Team –
I'm re-writing this section of the notes,
so if you were planning to transcribe this chapter from
the PDF notes, you can skip this one :)
– Freddie
-->

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML"
></script>
<script type="text/javascript" src="tutorialSheetScripts.js"> </script>
<link rel="stylesheet" type="text/css" media="all" href="styles.css">

## [Return to Contents](notes-contents)

<img src="figs/PDEchap.PNG" width="200"/>

# Chapter 14 - Partial Differential Equations
## Recap
In a previous chapter, we looked at
[ODEs – Ordinary Differential Equations](9-ODE).
Here we had a quantity of interest, a model for how the system evolves in time,
and a set of initial conditions.
One of the examples that we considered was the trolley on a spring.
Here the quantity of interest was the position of the trolley
at any moment in time $x(t)$,
the model for how it evolves was an ODE like
$\ddot{x}(t) = -\frac{k}{m} x(t)$,
and the initial conditions were $x(0)$ and $\dot{x}(0)$,
the position and velocity of the trolley at $t=0$, when we start the clock.

Solving the ODE meant finding an $x(t)$ that satisfied the ODE
in the general case (of any initial conditions), i.e.
$x(t) = A \cos(\omega t) + B \sin(\omega t)$
with
$\omega = \sqrt{\frac{k}{m}}$;
then matching this general solution to the particular initial conditions we
were given. i.e.
$x(t) = x(0) \cos(\omega t) + \frac{\dot{x}(0)}{\omega}\sin{\omega t}$.

Next, we considered
[Coupled Oscillators](10-coupled-oscillators),
where the game was the same but we had multiple discrete quantities of interest,
and again a model for how the system evolves and set of initial conditions.
E.g. for a set of trolleys connected to each other with springs.

In this setup we upgraded some of our quantities to vectors and matrices, e.g.
$x(t) \rightarrow \mathbf{x}(t)$,
and our ODE became,
$$
\ddot{\mathbf{x}}(t) =
  \begin{bmatrix}
    -\frac{k_1 + j}{m_1} & \frac{j}{m_1} \\
    \frac{j}{m_2} & -\frac{k_2 + j}{m_2}
  \end{bmatrix}
\mathbf{x}
$$
We solved this system by finding the *normal modes* that oscillate
in harmonic motion at a single frequency,
and added the combination of all solutions together for the
genaral solution, before matching to initial conditions.

### Upgrading to PDEs
**Partial Differential Equations**, PDEs, are the next step in this progression
where we go from modelling the time evolution of single quantity,
to a discrete set of quantities, to a continuous set of values – e.g.
the displacement of every point on a guitar string when it is struck,
the temperature at every point in a room when a heat source is nearby,
the stresses on the body of an aircraft during operation.

Solving PDEs allows us to calculate how our systems evolve in time,
but also can give us insight into how to optimise them e.g.
for their response efficiency, or to detect failure modes.

PDEs find applications in many fields of Engineering, and are studied in their
own right as a topic in Appled Mathematics.
In this module we will introduce the topic and give a first set of tools for
approaching them.

We'll be relying heavily on the intuition you have built up for ODEs,
so make sure you are refreshed and comfortable with that topic.

## Introducing PDEs
Returning to our analogy of ODEs and Coupled Oscillators,
Our quantity of interest is usually the value of a quantity
both at some moment in time and now also at some point in space.
i.e. a multivariate function of space and time – $f(\mathbf{x}, t)$.

The model for our system, how our quantity of interest evolves in time,
is no longer an *Ordinary* Differential Equation,
but a *Partial* Differential Equation,
i.e. a differential equation that contains
[partial derivatives](13-multivariate-calculus).

Some physically motivated PDEs that we'll look at in more detail are the
diffusion equation:
$$
\frac{\partial f(x, t)}{\partial t} =
\alpha \frac{\partial^2 f(x, t)}{\partial x^2}
$$
and the wave equation:
$$
\frac{\partial^2 f(x, t)}{\partial t^2} =
c^2 \frac{\partial^2 f(x, t)}{\partial x^2}
$$
Notice that in each we have deriviatives in both space and time of our quantity
of interest, $f(x, t)$,
as well as some constants of the system, $\alpha$ and $c$.

A small slight-of-hand to be aware of here is,
previously our quantity of interest was labeled $x$, and we solved for $x(t)$.
In this case, our quantity of interest is $f$
and now $x$ is one of our independent variables along with $t$,
and we solve for $f(x, t)$.
In different applications, we may use different symbols for our dependent and
independent quantities, so it's always worth noting which variables play what
role.

The initial conditions of the system also upgrade,
instead of a single value when $t=0$,
we now typically specify an entire function, $f(x, 0)$ –
i.e. what the system looks like at a snapshot in time.

Solving partial differential equations amounts to asking the question:

*"If I have a system who's behaviour is governed by a particular PDE,*
*and the system starts with a known initial state:*
*What is the state of the system at future times?"*

## 1D Wave Equation Example
In general, PDEs are difficult to solve, we're going to focus primarily on
two special cases that have special relevance in engineering
and are tractable to solve.
Though the tools we develop here will allow you to analyse
wider class of problems.

*The wave equation* is a class of PDEs that model vibration and wave phenomena.
This can be applied to multiple physical systems from
mechanical vibrations in solid bodies,
sound waves in accoustic systems,
and electromagnetic waves – i.e. light.

We saw a wave equation earlier in this chapter that looked like this,
$$
\frac{\partial^2 f(x, t)}{\partial t^2} =
c^2 \frac{\partial^2 f(x, t)}{\partial x^2}
$$
Let's see what we can do with an equation like this.

One question we may wish to answer when we have a PDE is,
*"Does a particular function that I have solve this PDE?"*.
If we have a candidate function $f(x, t) = \operatorname{sech}(x - a t)$,
where $a$ is a constant term,
let's determine if this solves the PDE
(we can discuss how we would get such a function later)

First thing to do is to calculate the derivatives of $f(x, t)$
that appear in the PDE.
In this case, the second derivatives in space and time.
(This can be done by hand, but
[Wolfram Alpha](https://www.wolframalpha.com/input/?i=D[Sech[x-a+t]%2C+{x,2}])
is pretty good for this too.)
$$
\frac{\partial f(x, t)}{\partial x} =
    -\tanh(x - a t)\operatorname{sech}(x - a t) \\
\frac{\partial^2 f(x, t)}{\partial x^2} =
    \operatorname{sech}(x - a t) - 2\operatorname{sech}^3(x - a t) \\
\frac{\partial f(x, t)}{\partial t} =
    a \tanh(x - a t)\operatorname{sech}(x - a t) \\
\frac{\partial^2 f(x, t)}{\partial t^2} =
    a^2 \operatorname{sech}(x - a t) - 2 a^2 \operatorname{sech}^3(x - a t)
$$
Then if we insert these derivatives into the PDE,
we can cancel things down a lot,
$$
a^2 \operatorname{sech}(x - a t) - 2 a^2 \operatorname{sech}^3(x - a t) =
c^2 \left(\operatorname{sech}(x - a t) - 2\operatorname{sech}^3(x - a t)\right)
$$
which reduces to,
$$
a^2 = c^2
$$
This says we have a condition on
$f(x, t) = \operatorname{sech}(x - a t)$ being a solution.
It *is* a solution, if and only if $a^2 = c^2$, i.e., $a = \pm c$.
Physically, this tells us the speed of the function,
that the $\operatorname{sech}$ envelope travels at speed $c$,
which was one of the constants of the PDE.
<iframe
  src="https://www.desmos.com/calculator/jk0daqq6df"
  width="800"
  height="600"
  style="border: 1px solid #ccc"
  frameborder=0
></iframe>

Here then we have two solutions to the wave equation,
$f(x, t) = \operatorname{sech}(x - c t)$ and
$f(x, t) = \operatorname{sech}(x + a t)$.
The wave equation is a *linear* differential equation,
which means we can combine any two solutions to form a third new solution –
recall how this was the case with ODEs as well.
So a more general solution to the PDE is,
$$
f(x, t) = A\operatorname{sech}(x - c t) + B\operatorname{sech}(x + c t)
$$
This is good, and we've made progress,
however we're not yet confident we've found *all* solutions –
what if our function doesn't look like $\operatorname{sech}$?
The following functions also solve the wave equation,
$$
f(x, t) = e^{-(x - c t)^2 / w^2} \\
f(x, t) = \frac{1}{w^2 + x^2 + 2 x c t + c^2 t^2}
$$
where $w$ is a free parameter that controls the width.
Try yourself by inserting these into the differential equation.

Since these solve the PDE with any value of parameter, $w$,
then any sum of solutions is also a solution, e.g.,
$$
f(x, t) = \sum_w A_w e^{-(x - c t)^2 / w^2} +
B_w \frac{1}{w^2 + x^2 + 2 x c t + c^2 t^2}
$$
for a set of $w$ values.

But again here, the solutions were given to us to begin with.
It would be good to have a more systematised way to approach the PDEs.

(It turns out with the  1D wave equation that any function,
$f(x, t) = f_+(x - c t) + f_-(x + c t)$,
is a solution, for arbitrary $f_+(x)$ and $f_-(x)$.
This is a special case though, we need something that will work in general.)

## Separation of variables
Let's trial another solution to the wave equation,
$$
f(x, t) = \cos k x \cos \omega t
$$
Here the parameter $k$ is called the *wavevector* and it determines the
wavelength of the cosine, $l = 2\pi / k$;
the parameter $\omega$ is the angular frequency (or informally just frequency)
and determines the time period, $\tau = 2\pi / \omega$.
If we insert this into the wave equation, we get the result,
$$
\frac{\partial^2 }{\partial t^2} \cos k x \cos \omega t =
c^2 \frac{\partial^2}{\partial x^2} \cos k x \cos \omega t
\\
-\omega^2 \cos k x \cos \omega t =
- c^2 k^2 \cos k x \cos \omega t
$$
And therefore,
$$
\omega^2 = c^2 k^2
$$
This resulting equation is called the *dispersion relation*,
which sets the relationship between the frequency and the wavevector that is
the condition for $f(x, t)$ to be a solution to the wave equation.
It says that a sinusoidal wave with a specific wavelength has a fixed period
that is related to it.

Let's look into our solution and see if
there are any general lessons we can learn.
Our trial solution, $f(x, t) = \cos k x \cos \omega t$,
had relatively simple derivatives.
Part of the reason for this is the part of the expression that varies
in $x$, i.e. $\cos k x$, and the part that varies in $t$, i.e. $\cos \omega t$,
are multiplied together, rather than mixed in a more intricate manner.

### Procedure

This is a key insight, we can look for solutions where the variables,
$x$ and $t$, are separated in multiplied terms.
Let's imagine we have a solution to the wave equation, $f(x)$,
which is the product of a part that only varies in $x$, let's call that part
$X(x)$, and a part that only varies in $t$, lets call that $T(t)$,
then we express our solution as $f(x) = X(x)T(t)$.

What happens if we plug this into the wave equation.
$$
\frac{\partial^2 f(x, t)}{\partial t^2} =
c^2 \frac{\partial^2 f(x, t)}{\partial x^2}
\\
\frac{\partial^2}{\partial t^2} X(x)T(t) =
c^2 \frac{\partial^2}{\partial x^2} X(x)T(t)
$$
Now the partial derivatives only *see* their respective terms, i.e.,
$$
X(x) \left(\frac{\partial^2}{\partial t^2} T(t)\right) =
c^2 \left(\frac{\partial^2}{\partial x^2} X(x) \right)T(t)
$$
which we can shorthand to,
$$
X(x)T''(t) = c^2 X''(x)T(t)
$$
If we indulge a final step in mathematical manipulation,
we can divide both sides by $X(x)T(t)$,
$$
\frac{X(x)T''(t)}{X(x)T(t)} = c^2 \frac{X''(x)T(t)}{X(x)T(t)}
$$
which if we cancel terms returns,
$$
\frac{T''(t)}{T(t)} = c^2 \frac{X''(x)}{X(x)}
$$
Let's look at what we have, one of our terms,
$T''(t) / T(t)$,
is only dependant on $t$.
The other term, $X''(x) / X(x)$, is only dependant on $x$.
The big step conceptually from here is to recognise that since the $t$ term
must be equal to a set of other terms that don't involve $t$,
the $t$ term itself must be independant of $t$, i.e. is a constant:
$$
\frac{T''(t)}{T(t)} = a \,,
$$
where $a$ is a constant. And this can be re-arranged into,
$$
T''(t) = a T(t)
$$
i.e. a linear ODE.

The same is true of the $x$ term, and we can get,
$$
X''(x) = b X(x)
$$
where $b$ is also a constant, with the relationship $a = c^2 b$ linking the two.

Let's pause and reflect here, because what we have done is to convert a PDE, which in principle is difficult to solve,
$$
\frac{\partial^2 f(x, t)}{\partial t^2} =
c^2 \frac{\partial^2 f(x, t)}{\partial x^2}
$$
into two ODEs, a relationship between the constants,
and a relationship between the solutions.
$$
T''(t) = a T(t) \\
X''(x) = b X(x) \\
a = c^2 b \\
f(x) = X(x)T(t)
$$
From here we can tackle the ODEs using the tools developed in
a [previous chapter](9-ODE).

Let's do just that.
Our ODEs here in $x$ and $t$ take the same form,
lets look at the $x$ one.
$$
X''(x) = b X(x)
$$
Reading the ODE, it is asking, what function,
when differentiated twice, returns itself times a constant?
Either exponential functions or hyperbolic trig functions
fit the bill here, and so do sines and cosines if $b$ is negative.
We're free to choose, let's look at sines and cosines.
Let's use the trial function,
$$
X(x) = \sin k x
$$
If we insert it into the ODE, we get,
$$
-k^2 \sin k x = b \sin k x
$$
or
$$
-k^2 = b
$$
i.e.¸
$$
X(x) = \sin \left(\sqrt{-b} x \right)
$$
The same is true of $X(x) = \cos \left(\sqrt{-b} x \right)$.
We could do the same with $t$ for,
$$
T(t) = \sin \left(\sqrt{a} t \right), \cos\left(\sqrt{a} t \right)
$$
Giving a combined solution that is the product of the separated 
solutions in a linear sum:
$$
f(x, t) =
\left[A \sin \left(\sqrt{-b} x \right) + B\cos \left(\sqrt{-b} x \right)\right]
\left[C \sin \left(\sqrt{-a} t \right) + D\cos \left(\sqrt{-a} t \right)\right]
$$
Which you can check solves the full PDE.

### Better constants
The square roots aren't very nice here, but since $a$ and $b$ are arbitrary constants, we can replace them with more meaningful symbols,
we used $k^2 = -b$ earlier as the coefficient of $x$, if we replace all instances of $b$ with $-k^2$, and similarly $a$ with $-\omega^2$,
We get as our solution,
$$
f(x, t) =
\left[A \sin \left(k x \right) + B\cos \left(k x \right)\right]
\left[C \sin \left(\omega t \right) + D\cos \left(\omega t \right)\right]
$$
Or if we use the dispersion relation, we get:
$$
f(x, t) =
\left[A \sin \left(k x \right) + B\cos \left(k x \right)\right]
\left[C \sin \left(k c t \right) + D\cos \left(k c t \right)\right]
$$
which means our for shorter wavelengths we have a higher frequency.

<iframe src="https://www.desmos.com/calculator/1bwugzm94f"
  width="800"
  height="600"
  style="border: 1px solid #ccc"
  frameborder=0
></iframe>

More crucially, if we replace our ODEs with,
$$
T''(t) = -\omega^2 T(t) \\
X''(x) = -k^2 X(x) \\
\omega^2 = c^2 k^2 \\
f(x) = X(x)T(t)
$$
which, if we had anticipated the result,
we could have called our arbitrary complex constants
$-k^2$ and $-\omega^2$ this way from the start.
Here $k$ and $\omega$ are the wavevector and (angular) frequency
that we met earlier.

Note how we have freedom to choose the names of our
arbitrary constants.
If we had set $T''(t) = \gamma^2 T(t)$,
show that this would return exponentially growing and decaying
solutions.
How would this change the dispersion relation?

Let's pause again and reflect.
We've managed to derive the sinusoidal solution that we guessed
earlier, but this time from first principles.

What was our process?
* Assume a solution that is seperable: $f(x, t) = X(x)T(t)$.
* Plug this into our PDE, and let the derivatives work on like terms.
* Divide the whole thing by $X(x)T(t)$ and collect like terms.
* Set these terms to be constant, to define ODEs and a dispersion relation.
* Solve the ODEs and combine the solutions together.

We'll see in the next section this working on a different set of PDEs.

## Diffusion Equation
*The diffusion equation* is for diffusion processes,
i.e. how a concentration of a substance spreads out through a medium.
This could be how Lithium ions in an energy fuel cell diffuse through a
porous medium.
It is also the equation that governs temperature, and how heat is transfered
through solids.
For this reason, the diffusion equation is also called the *Heat Equation*.