# RS-orbits
Compute group actions on Riemann surfaces up to braid and topological equivalence.

The input is the output of `RepresentativesEpimorphisms` from
[genvectors.mag](https://github.com/jenpaulhus/breuer-modified/blob/main/genvectors.mag):
the generating vectors of a group G and signature, up to simultaneous conjugation.

```
load "genvectors.mag";
load "orbits.mag";

data := RepresentativesEpimorphisms([0,2,3,7], PSL(2,7));
brd, top := FindOrbits(data);
PrintOrbits(data, brd, top);
```

`brd[k]` (resp. `top[k]`) is the smallest index of a vector in the braid (resp. topological)
class of `data[k]`. `OrbitsFromIds(brd)` turns these into a list of classes.

Files written by `AddMonodromy` in genvectors.mag can be processed directly:

```
OrbitsFromFile("grpmono05", "orbits05");
```

Only quotient genus 0 is supported. For g0 > 0, `RepresentativesEpimorphisms`
does not return a complete list of vectors up to simultaneous conjugation.

## Equivalences
For a signature [0; m_1, ..., m_r] with m_1 <= ... <= m_r:
* **braid**: orbits under the braid group elements that preserve the signature
  (so cone points of the same order may be permuted)
* **topological**: orbits under Aut(G) together with those braids

## Method
Vectors are compared through a canonical form under simultaneous conjugation.
Braid orbits are found by a breadth-first search over the conjugacy classes of
generating vectors, using generators of the braid subgroup that preserves the
signature. A braid move never changes the multiset of conjugacy classes in a
vector, so vectors whose class multiset is shared with no other vector are
skipped. Topological classes are then unions of braid classes along generators
of Out(G).
