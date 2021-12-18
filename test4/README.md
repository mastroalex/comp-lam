
### 18 dec

### Selected layer thickness

Thickness: 0.2 mmm

From:

> [Multi-axial damage and failure of medical grade carbonfibre reinforcedPEEK laminates: Experimental testing and computational modellin](https://doi.org/10.1016/j.jmbbm.2018.03.015)
> <br>Elizabeth Anne GallagherSteven LamorinièrePatrick McGarr

![](fig/2021-12-18-09-58-05.png)

### Unit of measure 

Pa and meters, so:

```mathematica
CompParameter = 
 mixrules[236*10^9, 0.2, 27.6*10^9, 4*10^9, 0.36, 
  4*10^9/(2 (1 + 0.36)), 0.62]
```


### Corrected `totalQ` calculation

There was an error on index updating. 
Corretion from:
```mathematica
index = layer[[i, 1]];
```

To 
```mathematica
index = index + layer[[i, 1]];
```

### Start test from requesting

- $\left[\alpha/-\alpha/30/-30/0_2\right]s$ with $\alpha\in\left[0^\circ;90^\circ\right]$

```mathematica
EE1 = CompParameter[[1]];
EE2 = CompParameter[[2]];
\[Nu]\[Nu]12 = CompParameter[[3]];
GG12 = CompParameter[[4]];
alpha = 0;
t = 0.0002;
layer = {
   (* n di layer, angolo, E1, E2, \[Nu]12,G12,
   spessore*)
   {1, alpha, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, -alpha, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, 30, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, -30, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {2, 0, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {2, 0, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, -30, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, 30, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, alpha, EE1, EE2, \[Nu]\[Nu]12, GG12, t},
   {1, -alpha, EE1, EE2, \[Nu]\[Nu]12, GG12, t}
   };
```

Material proprieties was calcuated by `mixrules[]`. 

Different layer properties are in `layer` variable. By passing `layer` to `totalQ` this returns $\mathbb Q\mathbb Q[i],zzv[i],\theta\theta[i]$$ where `i` is the different layer index.  

To test:
```mathematica
{\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ], 
   zzv, \[Theta]\[Theta]} = 
  total\[DoubleStruckCapitalQ][0, layer, "homogenlayer" -> False];
MatrixForm[\[DoubleStruckCapitalQ]\[DoubleStruckCapitalQ]]
```

For this first test:

$\left(
\begin{array}{ccc}
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
 \{1.4858\times 10^{11},2.83747\times 10^9,0\} & \{2.83747\times 10^9,1.08799\times 10^{10},0\} & \{0,0,4.23843\times 10^9\} \\
\end{array}
\right)$

#### Laminates properties

In original files there was $L=10$ and $t=L/10$ now i redefined $t=0.2$ mm and so i can use $L=t*10$ ?

**I follow this idea**

So
```mathematica
(*Plate L x H x h *)
L = t*10;
H = L/2;
(*Surface normal load (positive if inward)*)
q3 = 0.02; (*force per \
unit area*)
(*Normal load applied on boundary at X=L (positive when \
aligned with global direction X)*)
q1 = 
 2 t; (* (force per unit area)*thickness = force per unit length *)
```

Now I define the properties of the structure as `L, H` and load.

Than compute A,B,D matrix:

![](fig/2021-12-18-10-10-45.png)

### Time to extract post processing info 

What can I extract ? 

* [ ] Vertical displacement of the unconstrained endpoint 
* [ ] Horizontal (both in plane) displacement of the unconstrained endpoint  
* [ ] Reaction force ? 


What load condiction ?

* [ ] `q3` normal load ? 
* [ ] `q1` vertical load ?

> ⚠️⚠️⚠️ I have to verify this information from the initial notebook and from the lecture notes