
# Adding fabric layer 

> 29 dec

From Kollar pg 42:

- Eq 2.140: $Q_{i j}^{\text {woven }}=\frac{1}{2}\left\lceil\left(\bar{Q}_{i j}\right)_{\Theta}+\left(\bar{Q}_{i j}\right)_{-\Theta}\right\rceil, \quad i, j=1,2,6$

![](pastedFig/2021-12-29-07-12-26.png =50%x)

Inside `totalQ` procedure, after `calcQ[]`calculation for each layer the $\mathbb Q$ matrix is passed to `CalcQ45Fabric` procedure to apply table 2.12 equations.

```mathematica
Calc\[DoubleStruckCapitalQ]45Fabric[\[DoubleStruckCapitalQ]_] := (
   \[DoubleStruckCapitalQ]2 = DiagonalMatrix[{0, 0, 0}];
   angolo = 45*Pi/180;
   c = Cos[angle];
   s = Sin[angle];
   \[DoubleStruckCapitalQ]2[[1, 1]] = 
    c^4 \[DoubleStruckCapitalQ][[1, 1]] + 
     s^4 \[DoubleStruckCapitalQ][[2, 2]] + 
     2 c^2 s^2 (\[DoubleStruckCapitalQ][[1, 2]] + 
        2 \[DoubleStruckCapitalQ][[3, 3]]);
   \[DoubleStruckCapitalQ]2[[2, 2]] = 
    s^4 \[DoubleStruckCapitalQ][[1, 1]] + 
     c^4 \[DoubleStruckCapitalQ][[2, 2]] + 
     2 c^2 s^2 (\[DoubleStruckCapitalQ][[1, 2]] + 
        2 \[DoubleStruckCapitalQ][[3, 3]]);
   \[DoubleStruckCapitalQ]2[[1, 2]] = 
    c^2 s^2 (\[DoubleStruckCapitalQ][[1, 
         1]] + \[DoubleStruckCapitalQ][[2, 2]] - 
        4 \[DoubleStruckCapitalQ][[3, 3]]) + (c^4 + 
        s^4) \[DoubleStruckCapitalQ][[1, 2]];
   \[DoubleStruckCapitalQ]2[[2, 1]] = \[DoubleStruckCapitalQ]2[[1, 2]];
   \[DoubleStruckCapitalQ]2[[3, 3]] =  
    c^2 s^2 (\[DoubleStruckCapitalQ][[1, 
         1]] + \[DoubleStruckCapitalQ][[2, 2]] - 
        2 \[DoubleStruckCapitalQ][[1, 2]]) + (c^2 - 
        s^2) \[DoubleStruckCapitalQ][[3, 3]];
   \[DoubleStruckCapitalQ]2[[1, 3]] = 0;
   \[DoubleStruckCapitalQ]2[[2, 3]] = 0;
   \[DoubleStruckCapitalQ]2[[3, 1]] = \[DoubleStruckCapitalQ]2[[1, 3]];
   \[DoubleStruckCapitalQ]2[[3, 2]] = \[DoubleStruckCapitalQ]2[[2, 
     3]] ;
   Return[\[DoubleStruckCapitalQ]2];
   );
```

Also tested with example:

![](pastedFig/2021-12-29-07-41-31.png =75%x)

This is file: `more_test_final_fabric.nb`
