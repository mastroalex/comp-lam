# Stress in thickness

I have to implement:

![](pastedFig/2021-12-22-10-34-59.png =75%x)

And obtain:

![](pastedFig/2021-12-22-10-35-32.png =75%x)

## Otain stress and curvatures

Compiled function:

```mathematica
PostStrainCurvature[quantity_] := (
   If[quantity == "strain",
    Return[{
      SMTPostData["\[Epsilon]ox", Point[{L/2, H/2, 0}]],
      SMTPostData["\[Epsilon]oy", Point[{L/2, H/2, 0}]], 
      SMTPostData["\[Gamma]oxy"
       , Point[{L/2, H/2, 0}]] 
      }]
    ];
   If[quantity == "curvatures",
    Return[{
      SMTPostData["\[Kappa]x", Point[{L/2, H/2, 0}]] ,
      SMTPostData["\[Kappa]y", Point[{L/2, H/2, 0}]] ,
      SMTPostData["\[Kappa]xy", Point[{L/2, H/2, 0}]] 
      }]
    ];
   );
```

![](pastedFig/2021-12-22-10-45-11.png =50%x)

This allow me to extract:

```mathematica
\[Epsilon]post = PostStrainCurvature["strain"]
\[Kappa]post = PostStrainCurvature["curvatures"]
```

![](pastedFig/2021-12-22-10-46-14.png =50%x)

## Create $\underline  \varepsilon +z\underline  k$

Simple function:

```mathematica
\[Epsilon]FINCalc[t_] := (
   \[Epsilon]post = PostStrainCurvature["strain"];
   \[Kappa]post = PostStrainCurvature["curvatures"];
   \[Epsilon]FIN = \[Epsilon]post + t \[Kappa]post;
   Return[\[Epsilon]FIN]
   );
```

![](pastedFig/2021-12-22-10-50-45.png =50%x)

## Plot 
 
 Like ?

![](pastedFig/2021-12-22-10-58-13.png =50%x)

## What to plot ? $σ_x$, $σ_y$ o $σ_{xy}$  ?
SOME PROBLEM: 

![](pastedFig/2021-12-22-11-18-43.png)