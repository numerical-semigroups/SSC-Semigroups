# `NumericalSgps`
## Notable elements


In order to work with numerical semigroups, we have to load the package `numericalsgps`. For this tutorial we have used the development version available in [bitbucket](https://bitbucket.org/gap-system/numericalsgps).

```
gap> LoadPackage("num");  
```


We start by defining a numerical semigroup. There are many ways to do this depending on the information we have about it. If we know a system of generators, then we can do the following. We will store the semigroup in a variable called `s`.

```
gap> s:=NumericalSemigroup(3,5,7);
<Numerical semigroup with 3 generators>
```

Indeed, $\{3,5,7\}$ is a minimal generating set for `s`.
```
gap> MinimalGenerators(s);
[ 3, 5, 7 ]
```

Membership can be detected with `in`:
```
gap> 1 in s;
false
gap> 100 in s;
true
```

The set of sporadic elements (elements up to the conductor, or small elements) of the semigroup are
```
gap> SmallElements(s);
[ 0, 3, 5 ]
gap> Conductor(s);
5
gap> FrobeniusNumber(s);
4
```
This also is showing us the sets of gaps of `s`

```
gap> Gaps(s);
[ 1, 2, 4 ]
gap> Genus(s);
3
```

Recall that from the Apéry sets, one can retreive all this information.
```
gap> AperyList(s);
[ 0, 7, 5 ]
```
If only the semigroup is given as argument, then the Apéry set is computed with respect to the multiplicity. Note that the output is a list, in the $i$th position we find the smallest element in `s` congruent with $i$ modulo the multiplicity (or the given element we specify as second argument).
```
gap> AperyList(s,10);
[ 0, 11, 12, 3, 14, 5, 6, 7, 8, 9 ]
```
It is well know that if the second argument is in the semigroup, then the output will have length precisely this argument. It not, the we get some extra elements.
```
gap> AperyList(s,6);
[ 0, 7, 8, 3, 10, 5 ]
```
The set of pseudo-Frobenius numbers can be computed as follows.
```
gap> PseudoFrobeniusOfNumericalSemigroup(s);
[ 2, 4 ]
gap> Type(s);
2
```
Let us for instance check that Wilf's conjecture holds for all numerical semigroups with conductor equal to 20 (and thus Frobenius number 19).
```
gap> l:=NumericalSemigroupsWithFrobeniusNumber(19);;
gap> Length(l);
961
gap> ForAll(l, s->20<=EmbeddingDimension(s)*(Length(SmallElements(s))-1));
true
```
We are substracting one to the number of small elements since we consider the conductor as small element.

The special gaps (those that can be use to extend the semigroup to another numerical semigroup) are
```
gap> SpecialGaps(s);
[ 4 ]
gap> t:=AddSpecialGapOfNumericalSemigroup(4,s);
<Numerical semigroup>
gap> SmallElements(t);
[ 0, 3 ]
```
And the set of all oversemigroups:
```
gap> OverSemigroupsNumericalSemigroup(s);
[ <The numerical semigroup N>, <Numerical semigroup with 2 generators>,
  <Numerical semigroup with 3 generators>,
  <Numerical semigroup with 3 generators> ]
gap> List(last,MinimalGenerators);
[ [ 1 ], [ 2, 3 ], [ 3 .. 5 ], [ 3, 5, 7 ] ]
```

The set of fundamental gaps, is computed as follows. We can use fundamental gaps to define semigroups.
```
gap> FundamentalGaps(s);
[ 4 ]
gap> NumericalSemigroupByFundamentalGaps([4])=s;
true
```
As we can do with small elementa and gaps.
```
gap> NumericalSemigroupByGaps(Gaps(s))=s;
true
gap> NumericalSemigroupBySmallElements(SmallElements(s))=s;
true
```

## Irreducible numerical semigroups

Our semigroup example `s` is not symmetric, but it is pseudo-symmetric.
```
gap> IsIrreducible(s);
true
gap> IsSymmetric(s);
false
```
Symmetric numerical semigroups have type one, whole pseudo-symmetric have type equal to two.

```
gap> l:=IrreducibleNumericalSemigroupsWithFrobeniusNumber(19);;
gap> List(l,MinimalGenerators);
[ [ 2, 21 ], [ 3, 11 ], [ 4, 6, 17 ], [ 4, 9, 14 ], [ 4, 10, 13 ], [ 5, 6 ],
  [ 5, 8, 12 ], [ 5, 11, 12, 13 ], [ 6, 8, 9 ], [ 6, 8, 10, 15, 17 ],
  [ 6, 9, 11, 14, 16 ], [ 6, 10, 11, 14, 15 ], [ 7, 8, 9, 13 ],
  [ 7, 8, 10, 13 ], [ 7, 9, 11, 13, 15, 17 ], [ 7, 10, 11, 13, 15, 16 ],
  [ 8, 9, 12, 13, 14, 15 ], [ 8, 10, 12, 13, 14, 15, 17 ],
  [ 9, 11, 12, 13, 14, 15, 16, 17 ], [ 10, 11, 12, 13, 14, 15, 16, 17, 18 ] ]
```
Every numerical semigroup can be expressed (though not uniquely) as the intersection of irreducible numerical semigroups.

```
gap> t:=NumericalSemigroup(10,11,12,13);
<Numerical semigroup with 4 generators>
gap> DecomposeIntoIrreducibles(t);
[ <Numerical semigroup with 5 generators>,
  <Numerical semigroup with 6 generators>,
  <Numerical semigroup with 6 generators> ]
gap> List(last,MinimalGenerators);
[ [ 10, 11, 12, 13, 15 ], [ 10, 11, 12, 13, 18, 19 ],
  [ 10, 11, 12, 13, 19, 27 ] ]
```
We can actually find all oversemigroups of `t` that are irreducible.
```
gap> o:=OverSemigroupsNumericalSemigroup(t);;
gap> oi:=Filtered(o,IsIrreducible);;
gap> Length(oi);
35
gap> t=Intersection(oi);
true
```
There are some special families of symmetric numerical semigroups, as complete intersections, free, telescopic, ...
```
gap> s:=NumericalSemigroup(4,6,9);
<Numerical semigroup with 3 generators>
gap> IsFree(s);
true
gap> lf:=FreeNumericalSemigroupsWithFrobeniusNumber(19);;
gap> List(lf,MinimalGenerators);
[ [ 2, 21 ], [ 3, 11 ], [ 4, 6, 17 ], [ 4, 9, 14 ], [ 4, 10, 13 ], [ 5, 6 ],
  [ 5, 8, 12 ], [ 6, 8, 9 ] ]
```

## Almost symmetric numerical semigroups

We can construct almost symmetric numerical semigruops from irreducible numerical semigroups by removing some of its elements.
```
gap> AlmostSymmetricNumericalSemigroupsFromIrreducible(s);
[ <Numerical semigroup>, <Numerical semigroup with 4 generators>,
  <Numerical semigroup with 4 generators> ]
gap> List(last,MinimalGenerators);
[ [ 4, 6, 9 ], [ 4, 6, 13, 15 ], [ 4, 9, 10, 15 ] ]
```
Actually, one can compute with this idea the set of all almost symmetric numerical semigroups with given Frobenius number.
```
gap> la:=AlmostSymmetricNumericalSemigroupsWithFrobeniusNumber(19);;
gap> Length(la);
131
gap> t:=First(la,s->not(IsIrreducible(s)));
<Numerical semigroup>
gap> MinimalGenerators(t);
[ 4, 6, 21, 23 ]
gap> Genus(t)=(FrobeniusNumber(t)+Type(t))/2;
true
```

## Ideals

An ideal of a numerical semigroup can be defined by means of a generating system.

```
gap> s:=NumericalSemigroup(6,9,20);
<Numerical semigroup with 3 generators>
gap> i:=[2,3]+s;
gap> MinimalGenerators(i);
[ 2, 3 ]
```
The above ideal is not included in its ambient semigroup, and so it is not integral.
```
gap> IsIntegral(i);
false
gap> AmbientNumericalSemigroupOfIdeal(i);
<Numerical semigroup with 3 generators>
```
We can check membership with `in.`
```
gap> 8 in s;
false
gap> 8 in i;
true
```
Ideals also have conductors, since they are unions of translations of numerical semigroups.
```
gap> Conductor(i);
26
gap> SmallElements(i);
[ 2, 3, 8, 9, 11, 12, 14, 15, 17, 18, 20, 21, 22, 23, 24, 26 ]
```
The basic operations include intersection, sum, addition, translations and difference.
```
gap> j:=10+s;
<Ideal of numerical semigroup>
gap> i+j;
<Ideal of numerical semigroup>
gap> MinimalGenerators(i+j);
[ 12, 13 ]
gap> MinimalGenerators(10+i);
[ 12, 13 ]
gap> Difference(i,j);
[ 2, 3, 8, 9, 11, 12, 14, 15, 17, 18, 20, 21, 23, 24, 26, 27, 29, 32, 33, 35,
  38, 41, 44, 47, 53 ]
gap> MinimalGenerators(Intersection(i,j));
[ 22, 30 ]
```
The colon operation of ideals of rings translates in ideals of numerical semigroups to $I-J=\{z\in \mathbb{Z} \mid z+J\subseteq I\}$.
```
gap> MinimalGenerators(i-j);
[ -8, -7 ]
```
There are a couple of special ideals, one is the maximal ideal and the other the canonical (unique up to translations).
```
gap> m:=MaximalIdealOfNumericalSemigroup(s);
<Ideal of numerical semigroup>
gap> c:=CanonicalIdealOfNumericalSemigroup(s);
<Ideal of numerical semigroup>
gap> c-(c-i)=i;
true
gap> MinimalGenerators(m);
[ 6, 9, 20 ]
gap> MinimalGenerators(c);
[ 0 ]
gap> IsCanonicalIdeal(10+c);
true
```
The blow up of an ideal $I$ of a numerical semigroup $S$ is defined as $\bigcup_{n\in \mathbb{N}}(nI-nI)$.
```
gap> MinimalGenerators(BlowUpIdealOfNumericalSemigroup(i));
[ 0, 1, 2, 3, 4, 5 ]
gap> List([1..10], n->HilbertFunctionOfIdealOfNumericalSemigroup(n,i));
[ 4, 4, 3, 2, 2, 2, 2, 2, 2, 2 ]
```
If the ideal is the maximal ideal, then it coincides with the Lipman semigroup of $S$.
```
gap> MinimalGenerators(BlowUpOfNumericalSemigroup(s));
[ 3, 14 ]
gap> LipmanSemigroup(s);
<Numerical semigroup with 3 generators>
gap> MinimalGenerators(last);
[ 3, 14
```
One can keep computing Lipman semigroups until we reach $\mathbb{N}$, and the multiplicities of these semigroups conform the multiplicity sequence of $S$
```
gap> MultiplicitySequenceOfNumericalSemigroup(s);
[ 6, 3, 3, 3, 3, 2, 1 ]
```
## Minimal presentations

The set of factorizations of an element in a numerical semigroup can be obtained as follows.

```
gap> s:=NumericalSemigroup(6,9,20);
<Numerical semigroup with 3 generators>
gap> FactorizationsElementWRTNumericalSemigroup(36,s);
[ [ 6, 0, 0 ], [ 3, 2, 0 ], [ 0, 4, 0 ] ]
```
And the set of $\mathcal{R}$-classes as
```
gap> RClassesOfSetOfFactorizations(last);
[ [ [ 0, 4, 0 ], [ 3, 2, 0 ], [ 6, 0, 0 ] ] ]
```
The set of Betti elements is
```
gap> BettiElementsOfNumericalSemigroup(s);
[ 18, 60 ]
```
And a minimal presentation is obtained with
```
gap> MinimalPresentationOfNumericalSemigroup(s);
[ [ [ 1, 6, 0 ], [ 0, 0, 3 ] ], [ [ 3, 0, 0 ], [ 0, 2, 0 ] ] ]
```
Recall that we can produce this also by using elimination.
```
gap> x:=Indeterminate(Rationals,"x");;  
gap> y:=Indeterminate(Rationals,"y");;
gap> z:=Indeterminate(Rationals,"z");;
gap> t:=Indeterminate(Rationals,"t");;
gap> R:=PolynomialRing(Rationals,[x,y,z,t]);
Rationals[x,y,z,t]
gap> i:=Ideal(R,[x-t^6,y-t^9,z-t^20]);
<two-sided ideal in Rationals[x,y,z,t], (3 generators)>
gap> GroebnerBasis(i,EliminationOrdering([t]));
[ x^3-y^2, x*y^6-z^3, y^8-x^2*z^3, -x^2*y+z*t, y^5*t-x*z^2, x^2*y^3*t-z^2,
  y^2*t^2-z, y*t^3-x^2, x*t^3-y, t^6-x ]
```
The graph $G_n$ can be calculated in the following way.
```
gap> GraphAssociatedToElementInNumericalSemigroup(60,s);
[ [ 6, 9, 20 ], [ [ 6, 9 ] ] ]
```
In this example, it has three vertices and a single edge.

We can also calculate the shaded set of an element.
```
gap> ShadedSetOfElementInNumericalSemigroup(60,s);
[ [  ], [ 6 ], [ 6, 9 ], [ 9 ], [ 20 ] ]
```
The Hilbert series (or generating function of $S$) is computed via the polynomial associated to $S$.
```
gap> HilbertSeriesOfNumericalSemigroup(s,x);
(x^44-x^43+x^38-x^37+x^35-x^34+x^32-x^31+x^29-x^28+x^26-x^25+x^24-x^22+x^20-x^19+x^18-x^16+x^15-x^13+x^12-x^10+x^9-x^7+x^6-x+1)/(-x+1)
gap> NumericalSemigroupPolynomial(s,x);
x^44-x^43+x^38-x^37+x^35-x^34+x^32-x^31+x^29-x^28+x^26-x^25+x^24-x^22+x^20-x^19+x^18-x^16+x^15-x^13+x^12-x^10+x^9-x^7+x^6-x+1
```
Actually, this polynomial has all its roots in the unit circle.
```
gap> IsKroneckerPolynomial(last);
true
```
## Nonunique factorization invariants

We already know how to compute the set of factorizations of an element in the semigroup. The set of lengths is calculated as follows.

### Sets of lengths

```
gap> LengthsOfFactorizationsElementWRTNumericalSemigroup(100,s);
[ 5, 9, 10, 11, 12 ]
```

### Elasticity

The elasticity of an element and of the whole numerical semigroup can be computed with the following functions.
```
gap> ElasticityOfFactorizationsElementWRTNumericalSemigroup(100,s);
12/5
gap> ElasticityOfNumericalSemigroup(s);
10/3
```

### Delta sets

The set of distances or Delta set:
```
gap> DeltaSetOfFactorizationsElementWRTNumericalSemigroup(100,s);
[ 1, 4 ]
```
Or even of the whole semigroups
```
gap> DeltaSetOfNumericalSemigroup(s);
[ 1 .. 4 ]
```
Recall that the maximum is reached in the Betti elements of the semigroup.
```
gap> List(B,b->DeltaSetOfFactorizationsElementWRTNumericalSemigroup(b,s));
[ [ 1 ], [ 1, 4 ] ]
```
### Catenary degree

The catenary degree of a numerical semigroup is achieved in one of its Betti elements.

```
gap> List(B, b->CatenaryDegreeOfElementInNumericalSemigroup(b,s));
[ 3, 7 ]
gap> CatenaryDegreeOfNumericalSemigroup(s);
7
```

### Omega primality

The omega primality of an element or of the semigroup (maximum of its atoms) can be computed as follows.
```
gap> OmegaPrimalityOfElementInNumericalSemigroup(100,s);
20
gap> OmegaPrimalityOfNumericalSemigroup(s);
10
```
Because for the atoms we obtain
```
gap> A:=MinimalGenerators(s);
[ 6, 9, 20 ]
gap> List(A, a->OmegaPrimalityOfElementInNumericalSemigroup(a,s));
[ 3, 3, 10 ]
```

## Tame degree

We have specific funtions to compute the tame degree of an element in a numerical semigroup and the tame degree of the whole semigroup.

```
gap> TameDegreeOfElementInNumericalSemigroup(100,s);
7
gap> TameDegreeOfNumericalSemigroup(s);
10
gap> l:=Intersection(s,[1..100]);
[ 6, 9, 12, 15, 18, 20, 21, 24, 26, 27, 29, 30, 32, 33, 35, 36, 38, 39, 40,
  41, 42, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60,
  61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79,
  80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98,
  99, 100 ]
gap> List(l,x->TameDegreeOfElementInNumericalSemigroup(x,s));
[ 0, 0, 0, 0, 3, 0, 0, 3, 0, 3, 0, 3, 0, 0, 0, 3, 3, 0, 0, 0, 3, 3, 3, 0, 3,
  3, 0, 3, 0, 0, 0, 3, 0, 3, 0, 3, 0, 10, 0, 3, 3, 3, 3, 10, 3, 3, 10, 3, 0,
  10, 0, 3, 10, 3, 0, 10, 0, 7, 10, 3, 3, 10, 3, 7, 10, 3, 7, 10, 0, 7, 10,
  3, 0, 10, 0, 3, 10, 7 ]
```

## Random

Looking for a four generated numerical semigroup with 7 relators.

```
gap> l:=List([1..20], _->RandomNumericalSemigroup(5,500));;
gap> ll:=Filtered(l, s->EmbeddingDimension(s)=4);;
gap> Filtered(ll, s->Length(MinimalPresentationOfNumericalSemigroup(s))=7);
gap> MinimalGenerators(last[1]);
[ 315, 318, 485, 493 ]
```
