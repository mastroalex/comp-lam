# Debug some error
## Expecially the strange deformation of $[-30/-45/-30/-45]$

See [19-12 result](../more_test/README.md)

# Testing some case from Kollar.

## pg 86 $[0_{10}/45_{10}]$ 

Result:
$\left(\begin{array}{ccc}
194.525 & 39.4633 & -34.7917 \\
39.4633 & 55.3582 & -34.7917 \\
-34.7917 & -34.7917 & 42.7391
\end{array}\right),\left(\begin{array}{ccc}
-51.6112 & 16.8196 & -17.3958 \\
16.8196 & 17.9721 & -17.3958 \\
-17.3958 & -17.3958 & 16.8196
\end{array}\right),\left(\begin{array}{cccc}
64.8416 & 13.1544 & -11.5972 \\
-11.5972 & -11.5972 & 14.2464
\end{array}\right)$

> ERROR !!
> See Next case

Tested also dual case:
$[0_{10}/-45_{10}]$ 

Result:
$\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{lll}
194.525 & 39.4633 & 34.7917 \\
39.4633 & 55.3582 & 34.7917 \\
34.7917 & 34.7917 & 42.7391
\end{array}\right),\left(\begin{array}{ccc}
-51.6112 & 16.8196 & 17.3958 \\
16.8196 & 17.9721 & 17.3958 \\
17.3958 & 17.3958 & 16.8196
\end{array}\right),\left(\begin{array}{llll}
64.8416 & 13.1544 & 11.5972 \\
11.5972 & 11.5972 & 14.2464
\end{array}\right)\right\}$

# $[0_2/45_2/0_2/45]$

Result:
$\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{ccc}
77.8099 & 15.7853 & -13.9167 \\
15.7853 & 22.1433 & -13.9167 \\
-13.9167 & -13.9167 & 17.0956
\end{array}\right),\left(\begin{array}{ccc}
-4.1289 & 1.34556 & -1.39167 \\
1.34556 & 1.43777 & -1.39167 \\
-1.39167 & -1.39167 & 1.34556
\end{array}\right),\left(\begin{array}{cc}
4.14986 & 0.841883 & -0.742222 \\
0.841883 & 1.18097 & -0.742222 \\
-0.742222 & -0.742222 & 0.911768
\end{array}\right)\right\}$

In both case there are different error over the sign of $\mathbb A$ coefficient like:
- $\mathbb A_{31}$
- $\mathbb A_{32}$
- $\mathbb A_{13}$
- $\mathbb A_{23}$

Also in:
- $\mathbb B_{31}$
- $\mathbb B_{32}$
- $\mathbb B_{13}$
- $\mathbb B_{23}$

And:
- $\mathbb C_{31}$
- $\mathbb C_{32}$
- $\mathbb C_{13}$
- $\mathbb C_{23}$

# Error correction:

## zzv calculation:

Correction inside `totalQ[]` into:
```mathematica
zzv = -zzv + (Abs[zzv[[1]] + zzv[[Length[zzv]]]]/2);
```

And the following code:
```mathematica
Do[zzv\[LeftDoubleBracket]k\[RightDoubleBracket]=zzv\
\[LeftDoubleBracket]k-1\[RightDoubleBracket]+zzv\[LeftDoubleBracket]k\
\[RightDoubleBracket],{k,2,Length[zzv]}];
```

Into:
```mathematica
Do[
  	Do[
   	\[Theta]\[Theta][[index + j]] = layer[[i, 2]];
           zzv[[index + j + 1]] = zzv[[index + j]] + layer[[i, 7]];
   	\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[index + j]] = 
    Calc\[DoubleStruckCapitalQ][layer[[i, 3]], layer[[i, 4]], 
     layer[[i, 5]], layer[[i, 6]]]
   	, {j, 1, layer[[i, 1]]}];
  index = index + layer[[i, 1]];
  , {i, 1, Length[layer]}];
```

# ! FAKE - IT WAS CORRECT 

# Repristinated all

# New correction:

```mathematica
Print["total thickness: ", 
  Total[Table[layer[[i, 7]]*layer[[i, 1]], {i, 1, Length[layer]}]]];
  ```

  ## Reverse vector:

  

## Error with $T_{\sigma}$ and $T_\varepsilon$

> See [request page](request.md)