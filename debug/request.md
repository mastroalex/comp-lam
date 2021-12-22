# Problema

Ho identificato il problema, risale alle matrici $T_\sigma$ e $T_\epsilon$.

In particolare mi è chiaro che sono definite come:
- $\mathbf{T}_{\sigma}=\left[\begin{array}{ccc}
c^{2} & s^{2} & 2 s c \\
s^{2} & c^{2} & -2 s c \\
-s c & s c & c^{2}-s^{2}
\end{array}\right]$
- $\mathbf{T}_{\varepsilon}=\left[\begin{array}{ccc}
c^{2} & s^{2} & s c \\
s^{2} & c^{2} & -s c \\
-2 s c & 2 s c & c^{2}-s^{2}
\end{array}\right]$

Il mio dubbio è nella definizione di $[\bar Q]$.

In particolare ho tre diverse formule:
- A lezione abbiamo visto: $\overline{\mathbb{Q}}_{k}=\mathbb{T}_{\sigma}\left(\theta_{k}\right) \mathbb{Q}_{k} \mathbb{T}_{\sigma}^{T}\left(\theta_{k}\right)$
- Con il prof. Vairo abbiamo visto: $\left[\mathbf{T}_{\sigma}\right]\left[\begin{array}{ccc}
Q_{11} & Q_{12} & 0 \\
Q_{21} & Q_{22} & 0 \\
0 & 0 & Q_{66}
\end{array}\right]\left[\mathbf{T}_{\varepsilon}\right]^{-1}=[\bar Q]$
- Kollar, eq n. 3.14: $[\bar Q]=[T_\sigma]^{-1} [Q][T_\epsilon]$


Che implementate forniscono risultati diversi:

1. Definisco le due matrici $T_\sigma$ e $T_\epsilon$
  
  ```mathematica
\[DoubleStruckCapitalT]\[Sigma][\[Theta]_] := {{Cos[\[Theta]]^2, 
    Sin[\[Theta]]^2, 2 Cos[\[Theta]] Sin[\[Theta]]},
   {Sin[\[Theta]]^2, 
    Cos[\[Theta]]^2, -2 Cos[\[Theta]] Sin[\[Theta]]},
   {-Cos[\[Theta]] Sin[\[Theta]], Cos[\[Theta]] Sin[\[Theta]], 
    Cos[\[Theta]]^2 - Sin[\[Theta]]^2}};
\[DoubleStruckCapitalT]\[Epsilon][\[Theta]_] := {{Cos[\[Theta]]^2, 
    Sin[\[Theta]]^2, Cos[\[Theta]] Sin[\[Theta]]},
   {Sin[\[Theta]]^2, Cos[\[Theta]]^2, -Cos[\[Theta]] Sin[\[Theta]]},
   {-2 Cos[\[Theta]] Sin[\[Theta]], 2 Cos[\[Theta]] Sin[\[Theta]], 
    Cos[\[Theta]]^2 - Sin[\[Theta]]^2}};
```


2. Calcolo $\bar Q$

```mathematica
(*lessons*)
MatrixForm[\[DoubleStruckCapitalT]\[Sigma][
   45*Pi/180] . {{148.78, 2.91, 0}, {2.91, 9.71, 0}, {0, 0, 4.55}} . 
  Transpose[\[DoubleStruckCapitalT]\[Sigma][45*Pi/180]]]
(*Vairo*)
MatrixForm[\[DoubleStruckCapitalT]\[Sigma][
   45*Pi/180] . {{148.78, 2.91, 0}, {2.91, 9.71, 0}, {0, 0, 4.55}} . 
  Inverse[\[DoubleStruckCapitalT]\[Epsilon][45*Pi/180]]]
(*Kollar*)
MatrixForm[
 Inverse[\[DoubleStruckCapitalT]\[Sigma][45*Pi/180]] . {{148.78, 2.91,
     0}, {2.91, 9.71, 0}, {0, 0, 
    4.55}} . \[DoubleStruckCapitalT]\[Epsilon][45*Pi/180]]
```
3. Output:

$\begin{aligned}
&\left(\begin{array}{ccc}
45.6275 & 36.5275 & -34.7675 \\
36.5275 & 45.6275 & -34.7675 \\
-34.7675 & -34.7675 & 38.1675
\end{array}\right)\\
&\left(\begin{array}{ccc}
45.6275 & 36.5275 & -34.7675 \\
36.5275 & 45.6275 & -34.7675 \\
-34.7675 & -34.7675 & 38.1675
\end{array}\right)\\
&\left(\begin{array}{lll}
45.6275 & 36.5275 & 34.7675 \\
36.5275 & 45.6275 & 34.7675 \\
34.7675 & 34.7675 & 38.1675
\end{array}\right)
\end{aligned}$

In particolare per gli esercizi del Kollar in Tab 3.9 (pg 86) sembra funzionare la versione del Kollar. 
Nel codice: `layer1`,`layer2` e `layer3`.

Sembra essersi risolto anche il problema nel mio test `layer4`.

In particolare avendo definito il caso $[-30/-45/-30/-45]$ ottengo le tue differenti soluzioni:

## Definizione del Kollar:

$\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{ccc}
26.9008 & 12.9385 & -15.8977 \\
12.9385 & 12.9841 & -10.0712 \\
-15.8977 & -10.0712 & 13.5937
\end{array}\right),\left(\begin{array}{ccccc}
-0.432014 & 0.0840978 & 0.0990496 \\
0.0840978 & 0.263819 & -0.192274 \\
0.0990496 & -0.192274 & 0.0840978
\end{array}\right),\left(\begin{array}{cc}
0.358677 & 0.172514 & -0.211969 \\
0.172514 & 0.173121 & -0.134283 \\
-0.211969 & -0.134283 & 0.181249
\end{array}\right)\right\}$

![](fig/2021-12-20-23-13-02.png =50%x )

## Definizione vista a lezione: 

$\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{lll}
26.9008 & 12.9385 & 15.8977 \\
12.9385 & 12.9841 & 10.0712 \\
15.8977 & 10.0712 & 13.5937
\end{array}\right),\left(\begin{array}{ccc}
-0.432014 & 0.0840978 & -0.0990496 \\
0.0840978 & 0.263819 & 0.192274 \\
-0.0990496 & 0.192274 & 0.0840978
\end{array}\right),\left(\begin{array}{lll}
0.358677 & 0.172514 & 0.211969 \\
0.172514 & 0.173121 & 0.134283 \\
0.211969 & 0.134283 & 0.181249
\end{array}\right)\right\}$

![](fig/2021-12-20-23-14-51.png =50%x )

> Quale è corretta ?


# Funzionamento del codice:

Definizione dei parametri materiali nella sezione `SIMULAZIONE`.

Running dalla sezione `SIMULAZIONE`.

![](../code.drawio.svg)


La risoluzione avviene tramite il ciclo:


```mathematica
Do[
  Print["layer", 
   Table[
    Subscript[layer[[i, j, 2]], layer[[i, j, 1]]], {j, 1, 
     Length[layer[[i]]]}]];
  (*MyGeometry[layer_,AxialLoad_,TransvLoad_]*)
  
  MyGeometry[layer[[i]], axialLoad[[i]], transvLoad[[i]]];
  	FEMModel[];
	Coordinate[];
  Solution[];
  Print[Show[
    SMTShowMesh["DeformedMesh" -> True, "Mesh" -> GrayLevel[0.9]],
    SMTShowMesh["FillElements" -> False, "BoundaryConditions" -> True,
      "Mesh" -> GrayLevel[0]]
    ]];
  displacement[[i]] = PostProcessMyDisplacement[layer[[i]]];
  layername[[i]] = Table[layer[[i, j, 2]], {j, 1, Length[layer[[i]]]}];
  , {i, 1, Length[layer]}];
  ```
  
  In particolare il problema risale alla parte  `MyGeoemtry` dove vengono presi i singoli layer come definiti e c'è un ciclo sui diversi valori:
1. `totalQ` calcola le matrici $[Q]$ e le ordina in un vettore dal livello inferiore al superiore. Analogamente per gli angoli $\theta\theta$ e lo spessore $zzv$.
2. L'output di `totalQ`viene passato a `ABDcomp1` che restituisce $\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}$. **Qui c'è il problema della definizione di $[\bar Q]$**
   

## Esempio 1

![](pastedFig/2021-12-22-08-43-03.png =75%x )

- Definizione Kollar: $\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{lll}
194.525 & 39.4633 & 34.7917 \\
39.4633 & 55.3582 & 34.7917 \\
34.7917 & 34.7917 & 42.7391
\end{array}\right),\left(\begin{array}{ccc}
-51.6112 & 16.8196 & 17.3958 \\
16.8196 & 17.9721 & 17.3958 \\
17.3958 & 17.3958 & 16.8196
\end{array}\right),\left(\begin{array}{llll}
64.8416 & 13.1544 & 11.5972 \\ 13.1544 & 18.4527 & 11.5972 \\
11.5972 & 11.5972 & 14.2464
\end{array}\right)\right\}$
- Definizione lezione:$\left(\begin{array}{ccc}
194.525 & 39.4633 & -34.7917 \\
39.4633 & 55.3582 & -34.7917 \\
-34.7917 & -34.7917 & 42.7391
\end{array}\right),\left(\begin{array}{ccc}
-51.6112 & 16.8196 & -17.3958 \\
16.8196 & 17.9721 & -17.3958 \\
-17.3958 & -17.3958 & 16.8196
\end{array}\right),\left(\begin{array}{ccc}
64.8416 & 13.1544 & -11.5972 \\
13.1544 & 18.4527 & -11.5972 \\
-11.5972 & -11.5972 & 14.2464
\end{array}\right)$

Risultati:

![](pastedFig/2021-12-22-08-47-33.png =500x )

In particolare i risultati corrispondono solo con la definizione del Kollar.

## Esempio 2

- Def. Kollar: $\left(\begin{array}{lll}
77.8099 & 15.7853 & 13.9167 \\
15.7853 & 22.1433 & 13.9167 \\
13.9167 & 13.9167 & 17.0956
\end{array}\right),\left(\begin{array}{lll}
-4.1289 & 1.34556 & 1.39167 \\
1.34556 & 1.43777 & 1.39167 \\
1.39167 & 1.39167 & 1.34556
\end{array}\right),\left(\begin{array}{ccc}
4.14986 & 0.841883 & 0.742222 \\
0.841883 & 1.18097 & 0.742222 \\
0.742222 & 0.742222 & 0.911768
\end{array}\right)$
- Def. Lezione: $\{\mathbb{A}, \mathbb{B}, \mathbb{D}\}:=\left\{\left(\begin{array}{ccc}
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


Risultati:

![](pastedFig/2021-12-22-08-49-39.png =75%x )


## Caso $[-30_{1}/-45_{1}/-30_{1}/-45_{1}]$

Forse continuo ad ottenere risultati particolari:

![](pastedFig/2021-12-22-08-57-32.png =50%x )

![](pastedFig/2021-12-22-08-57-42.png =50%x )



