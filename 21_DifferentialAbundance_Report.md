# mothur: Differential Abundance & Biomarker Discovery

## What this process does

After determining that our microbial communities are significantly different (via Beta Diversity and AMOVA), the next logical step is to figure out **which specific organisms** are driving those differences. 

Two common approaches inside mothur:
1. **`lefse` (Linear discriminant analysis Effect Size):** A biomarker discovery tool that finds OTUs that consistently explain the differences between two or more biological conditions (e.g., 'Early' vs. 'Late') and ranks them by effect size (LDA score).
2. **`corr.axes`:** Calculates the correlation (e.g., Spearman) between the relative abundance of each OTU and the PCoA axes we generated in the previous step. This tells us which OTUs are physically pulling the samples apart on the 2D plot.

---

## mothur output

### 1. LEfSe (Biomarker Discovery)

```
mothur > lefse(shared=final.opti_mcc.shared, design=mouse.time.design)

You did not provide a class, using time.

Comparing Late-Early:
0.03

Number of significantly discriminative features: 105 ( 119 ) before internal wilcoxon.
Number of discriminative features with abs LDA score > 2 : 105.

Output File Names: 
final.opti_mcc.0.03.lefse_summary
```

### 2. Correlating OTUs to PCoA Axes

```
mothur > corr.axes(axes=final.opti_mcc.thetayc.0.03.lt.ave.pcoa.axes, shared=final.opti_mcc.shared, method=spearman, numaxes=3)

...
Output File Names: 
final.opti_mcc.spearman.corr.axes
```

---

## Output files

Below is a look at the core files generated during this biomarker discovery step:

### 1. LEfSe Summary (`final.opti_mcc.0.03.lefse_summary`)

This file lists the OTUs that are significantly different between the classes in your design file, along with their LDA (Linear Discriminant Analysis) score. An LDA score `> 2.0` is typically considered a statistically significant biomarker.

| OTU | logMaxMean | Class | LDA | pValue |
|---|---|---|---|---|
| Otu004 | 4.95386 | Late | 4.3973 | 0.0004467 |
| Otu005 | 5.04111 | Late | 4.68856 | 0.0006052 |
| Otu006 | 4.87061 | Late | 4.20025 | 0.0002386 |
| Otu008 | 4.74926 | Early | 4.06713 | 0.0275023 |
| Otu010 | 4.61767 | Late | 4.22217 | 0.0019184 |

### 2. Spearman Correlation (`final.opti_mcc.spearman.corr.axes`)

This file shows how strongly the abundance of an OTU correlates with movement along our defined PCoA axes (`axis1`, `axis2`, etc.).

| OTU | axis1 | p-value (axis1) | axis2 | p-value (axis2) | axis3 | p-value (axis3) | length |
|---|---|---|---|---|---|---|---|
| Otu001 | 0.071930 | 0.760235 | 0.212281 | 0.367785 | -0.640351 | 0.003142 | 0.678444 |
| Otu002 | 0.233333 | 0.322199 | 0.329825 | 0.161715 | -0.503509 | 0.027966 | 0.645562 |
| Otu004 | -0.566667 | 0.011415 | -0.038596 | 0.869927 | -0.192982 | 0.412926 | 0.599869 |
| Otu005 | -0.794737 | 0.000048 | -0.247368 | 0.293950 | -0.231579 | 0.325851 | 0.863960 |

---

## Next Steps Outside of mothur

Congratulations! If you have followed the pipeline up to this point, you have completed the core sequence processing and statistical benchmarking inside mothur. For generating publication-quality figures, most researchers will now:
1. Export the `.taxonomy`, `.shared`, and `.design` (metadata) files out of the bioinformatics environment.
2. Import them into **R** using tools like the `phyloseq` or `microbiome` packages.
3. Use `ggplot2` to generate custom stacked bar charts, rarefaction curves, and PCoA scatterplots color-coded by treatment group.
