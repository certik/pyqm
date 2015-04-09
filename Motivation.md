# Introduction #

When we have large molecule, and we are interested only in some its properties, we don't want to calculate it all with full precision (and often this is not even possible), but we want to split it somehow and calculate uninteresting parst say with quasi-classic approach and interesting parts with QM. then there is a problem how to connect things.

The idea is to take some large molecule that is impossible to calculate using HF. Decompose it into interesting and non-interesting parts, represent non-interesting part with a pseudo-potential and then calculate interesting part using HF to obtain energies and electronic densitiy, etc.

There are (mostly non-free) programs to do most of the tasks, but there is no consensus on the approach in the world.

We'll try to develop our pseudopotential, that's why lpp is here.

All HF calculations are traditionally done with gaussian basis, that's why if we want to use any potential we have to represent it in suitable basis: gaussians. The pseudopotential we'll try to us is `|l1m1>V(r)<l2m2|`, so we need to calculate its matrix elements in gaussian basis (possible located on different centers).

Look into [lpp/Documentation](http://hg.sympy.org/pyqm/file/tip/lpp/Documentation/), type `make lpp.ps` and this is what needs to be done.

Related papers:

http://landau.phys.spbu.ru/~kirr/phys/papers/

## detailed plan ##

  * the goal as we keep in head is to have an f77 function which recieves on input all the parameters (l1,m2,l2,m,2 g1

&lt;params&gt;

, g2

&lt;params&gt;

) and returns a number.
  * for all this formulas could be derived (matrix elements are integrals) and we could do it by hand
  * but our goal is to also derive formulas with the help of sympy and to generate such routines
  * for this we write down what integrals we have and try to write routines with the help of sympy to get resultant formulas first.
  * so first we should try to calculate angular parts with sympy (done with maxima, and partly with sympy)
  * then we should derive more formulas for radial integrals, and do them with sympy too
  * then when we get resultant formulas, we should verify them with numerical integration first, then generate routines.

## more detailed plan ##

  * first we need to get spherical integrals full working with sympy. this was done with maxima, and is half done with sympy.

For this we see that full matrix element (2) is split into radial part and product of two spherical integrals (3). Then we analyze (3) and see it is a mix (5) of more primitive spherical integrals (4). Then we expand exponent with scalar radial product in (4) using bessel jl functions, and also we rewrite x<sup>nx*y</sup>nz\*z^nz into sum of real spherical harmonicz Zlm (8), and then we get fully expanded formula for primitive spherical integrals (9).