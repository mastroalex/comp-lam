# Obtain single code for test simulation

> 18 dec

Obtained single code for testing $\alpha=\{0,10,20,30,40,50,60,70,80,90\}^\circ$. 

In `first_test.nb`:

Complete code:
```mathematica
SimulationComplete[alpha_] := (
   displacement = Table[{0*i, 0, 0}, {i, 1, Length[alpha]}];
   Do[
    Print["\[Alpha]=", alpha[[i]]];
    MyGeometry[alpha[[i]]];
    	FEMModel[];
	Coordinate[];
    Solution[];
    displacement[[i]] = PostProcessMyDisplacement[alpha[[i]]];
    , {i, 1, Length[alpha]}];
   PrintMyDisplacement[displacement, alpha];
   );

alpha = {0, 10, 20, 30, 40, 50, 60, 70, 80, 90};
SimulationComplete[alpha]
```

![](fig/2021-12-18-17-59-29.png)

Now i divide it into 3 folder to compute:
- Only assial load
- Only transveral load
- Both

⚠️ I need another graph: extension!

First of all:
* [x] Extension graph. Update in `first_test2.nb` (*)
* [x] Compute the same code for the cylinder (**)
* [x] Start to write down report/report ToC (***)

Than I can divide into three folder (only for the plate) + the cylinder ones. (****)

____

## (*)

Update the code by adding in `PostProcessMyDisplacement[]`:
```mathematica
data4 = SMTPostData["ul", Line[{{L, 0, 0}, {L, H, 0}}, 100], 
   "OutputForm" -> "Points"];
nodalpoint4 = Table[0*i, {i, 1, Length[Transpose[data4][[2]]]}];
Do[
  nodalpoint4[[i]] = Transpose[data4][[2, i, 2]];
  , {i, 1, Length[Transpose[data4][[2]]]}];
graph4 = 
  ListLinePlot[Transpose[{nodalpoint4, Transpose[data4][[1]]}], 
   PlotLabel -> "U displacement over X==L", AxesLabel -> {"Y", "u"}];
(*Print[graph3];*)

Export[StringJoin[NotebookDirectory[], "U_Y_alpha=", ToString[alpha], 
   ".pdf"], graph4, "pdf"];
 (*  ... *)
displ4 = Transpose[{nodalpoint4, Transpose[data4][[1]]}];
Return[{displ1, displ2, displ3, displ4}]
```

And into `PrintMyDisplacement[]`:

```mathematica
grafico4 = 
  ListLinePlot[
   Table[displacement[[i, 4]], {i, 1, Length[displacement]}], 
   PlotLabel -> "U displacement over X==L", AxesLabel -> {"X", "y"}, 
   PlotLabels -> 
    Table[
     StringJoin["alpha=", ToString[alpha[[i]]]], {i, 1, 
      Length[alpha]}]];
Print[grafico4];
Export[StringJoin[NotebookDirectory[], "U_Y=", ".pdf"], grafico4, 
  "pdf"];
```

![](fig/2021-12-18-18-54-48.png)

# (**)

Also implemented code for Cylinder like:
```mathematica
CSimulationComplete[alpha_] := (
   displacement = Table[{0*i}, {i, 1, Length[alpha]}];
   Do[
    Print["\[Alpha]=", alpha[[i]]];
    MyGeometryCyl[alpha[[i]]];
    FEMModelCyl[];
    CoordinateCyl[];
    SolutionCyl[];
    displacement[[i]] = PostProcessMyDisplacementCyl[alpha[[i]]];
    , {i, 1, Length[alpha]}];
   CPrintMyDisplacement[displacement, alpha];
   );

alpha = {0, 10, 20, 30, 40, 50, 60, 70, 80, 90, -45};
CSimulationComplete[alpha]
```

Now I read vertical (`w`) displacement for superior and inferior border ($Z=R \:\&\: Z=-R$) and both later (`v`) displacement for later border ($Y=R\:\&\:Y=-R$).
For example:

![](fig/2021-12-18-22-28-11.png)

![](fig/2021-12-18-22-29-09.png)

# (***)
Starting write down report. Copied report template from [bone-homogenization](https://github.com/mastroalex/bone-homogenization) in [report folder](../report). 



# (****)

Divided into 
- Only assial load `first_test/axial_load/first_test_2_axial.nb`
- Only transveral load  `first_test/axial_load/first_test_2_transversal.nb`
- Both (original file) `first_test/first_test_2.nb`.

✅ TEST DONE !

## Transversal load: 

![](fig/2021-12-18-22-43-09.png)

## Axial load:

![](fig/2021-12-18-22-54-45.png)

## Both load:

![](fig/2021-12-18-22-42-44.png)


