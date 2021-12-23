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

## How to plot ? $σ_x$, $σ_y$ o $σ_{xy}$  ?
SOME PROBLEM: 

![](pastedFig/2021-12-22-11-18-43.png)

### Use `Pieciewise[]`

```mathematica
Do[
 plotval = 
   Join[plotval, {{(\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[
          i]] . \[Epsilon]FINCalc[x])[[1]], 
      zzv[[i]] < x < zzv[[i + 1]]}}];
 , {i, 1, Length[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]}
 ]
plotfun = Piecewise[plotval
   ];
MatrixForm[plotval]
xval
Plot[plotfun, {x, zzv[[1]], zzv[[Length[zzv]]]}]
```

![](pastedFig/2021-12-23-07-04-14.png =50%x)

### Transform into procedure:

```mathematica
plotStress[QQ_, zzv_] := (
   plotval = {};
   Do[
    plotval = 
      Join[
       plotval, {{(\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[
             i]] . \[Epsilon]FINCalc[x])[[1]], 
         zzv[[i]] < x < zzv[[i + 1]]}}];
    , {i, 1, Length[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]}
    ];
   plotfun = Piecewise[plotval];
   stressGraph = Plot[plotfun, {x, zzv[[1]], zzv[[Length[zzv]]]}];
   Print[stressGraph];
   );
```
### Problem: do not use $ [Q]$ but $[\bar Q]$ 

Update `Do[]`cycle as:
```mathematica
Do[
  plotval = 
    Join[
     plotval, {{((Inverse[\[DoubleStruckCapitalT]\[Sigma][\[Theta]\
\[Theta][[i]]]] . \[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[
             i]] . \
\[DoubleStruckCapitalT]\[Epsilon][\[Theta]\[Theta][[
              i]]]) . \[Epsilon]FINCalc[x])[[2]], 
       zzv[[i]] < x < zzv[[i + 1]]}}];
  , {i, 1, Length[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]}
  ];
```

Now it seem better:

![](pastedFig/2021-12-23-07-19-29.png =75%x)


## Final procedure:

```mathematica
plotStress[QQ_, zzv_, \[Theta]\[Theta]_, axis_] := (
   If[axis == "\[Sigma]x", index = 1];
   If[axis == "\[Sigma]y", index = 2];
   If[axis == "\[Tau]xy", index = 3];
   plotval = {};
   plotval1 = {};
   Do[
    plotval = 
      Join[
       plotval, {{((Inverse[\[DoubleStruckCapitalT]\[Sigma][\[Theta]\
\[Theta][[i]]]] . \[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[
               i]] . \[DoubleStruckCapitalT]\[Epsilon][\[Theta]\
\[Theta][[i]]]) . \[Epsilon]FINCalc[x])[[index]], 
         zzv[[i]] < x < zzv[[i + 1]]}}];
    , {i, 1, Length[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]}
    ];
   Do[
    plotval1 = 
      Join[
       plotval1, {{((Inverse[\[DoubleStruckCapitalT]\[Sigma][\[Theta]\
\[Theta][[i]]]] . \[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ][[
               i]] . \[DoubleStruckCapitalT]\[Epsilon][\[Theta]\
\[Theta][[i]]]) . {1, 1, 1})[[index]], 
         zzv[[i]] < x < zzv[[i + 1]]}}];
    , {i, 1, Length[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]}
    ];
   plotfun = Piecewise[plotval];
   plotfun1 = Piecewise[plotval1];
   stressGraph = 
    Plot[plotfun, {x, zzv[[1]], zzv[[Length[zzv]]]}, 
     Exclusions -> None, PlotLabel -> axis];
   qgraph1 = 
    Plot[plotfun1, {x, zzv[[1]], zzv[[Length[zzv]]]}, 
     Exclusions -> None, PlotLabel -> "\[DoubleStruckCapitalQ]"];
   epsilongraph1 = 
    Plot[(\[Epsilon]FINCalc[x])[[index]], {x, zzv[[1]], 
      zzv[[Length[zzv]]]}, Exclusions -> None, 
     PlotLabel -> "strain"];  
   Print[stressGraph];
   Print[qgraph1];
   Print[epsilongraph1];
   );
```
![](pastedFig/2021-12-23-07-50-55.png =75%x)
