# Laminates composites structural response 🧩
### Computational Mechanics of Tissues and Biomaterials
### Biomedical Engineering - University of Rome Tor Vergata

#### Abstract

In the present analysis several simulation campaigns were carried out to analyze the structural response of CF / PEEK laminated composites showing great potential for application in the biomedical field. 

<br> <strong> Background </strong>, in a laminated composite the structural behavior depends on the arrangement of the individual layers, in these analyzes it is investigated how the different layouts influence the response of some structural elements; </p>

<strong> Results </strong>, different simulation campaigns show the variation of the stiffness and of the coupling conditions between the load conditions and the structural response to the variation of the layout and the grain angle of the foil; 

<strong> Conclusions </strong>, structural responses are highly variable, from simpler behaviors to predict to more complex responses. An analysis that takes into account the layout of the entire laminate, starting from the composition of the single layer, allows for precise results to understand the structural behavior.

**Read the [report](https://github.com/mastroalex/comp-lam/blob/main/report/main2.pdf)**

**Read more [website](https://alessandromastrofini.it/)**

---

 ![](https://github.com/mastroalex/comp-lam/blob/main/report/figures/laminate_figures.svg)
 
 ---
 
 ```mathematica
 << AceFEM ‘;
SimulationComplete[alpha_ , axialLoad_ , trasLoad_] := (
displacement = Table[{0*i, 0, 0}, {i, 1, Length[alpha]}];
Do[
Print["α=", alpha[[i]]];
MyGeometry[alpha[[i]], axialLoad , trasLoad];
FEMModel [];
Coordinate [];
Solution [];
Print[Show[SMTShowMesh["DeformedMesh" -> True, "Mesh" -> GrayLevel[0.9]], SMTShowMesh["
FillElements" -> False, "BoundaryConditions" -> True, "Mesh" -> GrayLevel[0]]]]; displacement[[i]] = PostProcessMyDisplacement[alpha[[i]]];,
{i, 1, Length[alpha]}];
PrintMyDisplacement[displacement , alpha]; ;
(***)
alpha = {0, 10, 20, 30, 40, 50, 60, 70, 80, 90};
axLoad = 2*10^3* (L/10);
trLoad = 0.02*10^3;
SimulationComplete[alpha, axLoad, trLoad]
```

 
## Note

- 14 dec - Officially extended code to accept different layer with different material and heigth. Test with first example on Kollar.
- [17 dec](test3/README.md) - Decided to use PEEK material. Done mixing rules homogenization. See [test3 Readme](test3/README.md)
- [18 dec](test4/README.md) - Optimized dimension and obtained post processing graphs. See also [first_test](first_test/README.md) for the unique test code. Complete first serie of test for $[-\alpha,\alpha,30,-30/0_2]_s$. Started [report](report/README.md)
- [19 dec](more_test/README.md) - Starting generalizing code to accept more cases. 
- [20 dec](more_test/README.md) - Added post processing visual information. Started some test. Started wrtiting report. Started some debug test --> see [debug folder](debug/README.md)
- [21 dec](debug/README.md) - Lits all question: [request](debug/request.md). Discovered markdown fig dimension comand and personalized command. 
- [22 dec](/debug/stress.md) - Talk with prof: the error was in the interpretation of the rotation matrix. Correct interpretation is **the one from Kollar**. Update all code! 
- 23 dec - Finished σ plotting, see [stress.md](debug/stress.md). Corrected file: [first_test folder](first_test/README.md) and done test.
- 24 dec - Corrected file: [more_Test folder](first_test/README.md) and done test.
- 29 dec - Continue testing. [See more_test README](more_test/FABRIC/README.md). Developed code for fabric layer (`more_test_final_fabric.nb`) and for last case of analysis (`more_test_final_last_layer.nb`).
- 30 dec - Added plot for last layer case.


