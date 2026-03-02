# mothur: Beta Diversity

**Commands:**

```
mothur > dist.shared(shared=final.opti_mcc.shared, calc=thetayc-jclass, subsample=2404)

mothur > pcoa(phylip=final.opti_mcc.thetayc.0.03.lt.dist)

mothur > nmds(phylip=final.opti_mcc.thetayc.0.03.lt.dist)

mothur > amova(phylip=final.opti_mcc.thetayc.0.03.lt.ave.dist, design=mouse.time.design)
```

---

## What this process does

While Alpha Diversity looks at the richness and evenness *within* a single sample, **Beta Diversity** compares the microbial communities *between* different samples to determine how similar or different they are.

1. **`dist.shared`:** This command calculates a pairwise distance matrix between all of our samples. It compares the OTUs that are shared vs. unshared between each pair of samples. We use the same `subsample` value as we did in Alpha diversity to ensure fair comparisons.
   - `jclass` (Jaccard): Looks purely at presence/absence of an OTU (community membership).
   - `thetayc` (Yue & Clayton): Takes into account both the presence/absence *and* the relative abundance of OTUs (community structure).
2. **`pcoa` / `nmds`:** These are ordination techniques that allow us to visualize the complex multi-dimensional distance matrix in 2D or 3D space. Samples that harbor similar microbial communities will cluster closely together, while samples with vastly different communities will be far apart.
3. **`amova`:** Analysis of Molecular Variance determines if the spatial separation (clustering) we see between different biological treatment groups is statistically significant. You will need a `.design` file that maps each sample (`F3D0`, `F3D1`, etc.) to its metadata treatment group.

---

## mothur output

### 1. Generating Distance Matrices

```
mothur > dist.shared(shared=final.opti_mcc.shared, calc=thetayc-jclass, subsample=2404)

Using 8 processors.
0.03 
100
...
125

It took 0 seconds to run dist.shared.

Output File Names: 
final.opti_mcc.thetayc.0.03.lt.ave.dist
final.opti_mcc.thetayc.0.03.lt.std.dist
final.opti_mcc.jclass.0.03.lt.ave.dist
final.opti_mcc.jclass.0.03.lt.std.dist
```

This creates several `.dist` files. The `.lt.ave.dist` files are the "lower-triangle average" distance matrices, which represent the average distance between samples across 1000 random subsampling iterations.

### 2. PCoA Visualization

```
mothur > pcoa(phylip=final.opti_mcc.thetayc.0.03.lt.ave.dist)

Processing...
Rsq 1 axis: 0.73444
Rsq 2 axis: 0.888178
Rsq 3 axis: 0.977583

Output File Names: 
final.opti_mcc.thetayc.0.03.lt.ave.pcoa.axes
final.opti_mcc.thetayc.0.03.lt.ave.pcoa.loadings
```

The resulting `.axes` file contains the X, Y, and Z coordinates for each sample, which can be plotted in Excel, R (using `ggplot2`), or Python.

### 3. AMOVA Statistical Testing

```
mothur > amova(phylip=final.opti_mcc.thetayc.0.03.lt.ave.dist, design=mouse.time.design)

Early-Late	Among	Within	Total
SS	0.624322	0.551173	1.17549
df	1	17	18
MS	0.624322	0.0324219

Fs:	19.2562
p-value: <0.001*

Experiment-wise error rate: 0.05
If you have borderline P-values, you should try increasing the number of iterations

Output File Names: 
final.opti_mcc.thetayc.0.03.lt.ave.amova
```

If the p-value is significant (e.g., < 0.05), we can confidently say that the microbial communities between our defined treatment groups are statistically different from one another!

---

## Output files

Below is a look at the core files generated during this analysis step:

### 1. The Distance Matrix (`final.opti_mcc.thetayc.0.03.lt.ave.dist`)

A lower-triangle matrix containing pairwise distance values (ranging from 0 to 1). A value of `0` means the samples are identical, while a value of `1` means they share absolutely no OTUs.

### 2. Ordination Axes (`final.opti_mcc.thetayc.0.03.lt.ave.pcoa.axes`)

*Excerpt:*

| group | axis1 | axis2 | axis3 |
|---|---|---|---|
| F3D0 | -0.150257 | -0.180184 | -0.016522 |
| F3D1 | 0.284032 | -0.341314 | -0.044761 |
| F3D141 | -0.126254 | 0.002137 | 0.011519 |
| F3D142 | -0.075463 | 0.078165 | 0.002861 |
| F3D143 | -0.210699 | 0.001207 | 0.029253 |

These columns map directly to points on a scatterplot.

---

## Next step

With Beta Diversity complete, you have finished the core sequence processing pipeline! The next steps generally involve external visualization and moving beyond mothur:
- **Plotting:** Bring the `.axes`, `.summary`, and `.taxonomy` files into **R** for customized plotting using `phyloseq`, `vegan`, or `ggplot2`.
- **Differential Abundance:** Run programs like **LEfSe** or **DESeq2** to find out *which specific OTUs* are driving the differences between our groups.
