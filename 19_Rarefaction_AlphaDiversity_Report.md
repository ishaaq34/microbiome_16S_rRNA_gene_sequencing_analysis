# mothur: Rarefaction & Alpha Diversity

**Commands:**

```
mothur > count.groups(shared=final.opti_mcc.shared)

mothur > summary.single(shared=final.opti_mcc.shared, calc=nseqs-coverage-sobs-invsimpson, subsample=2404)

mothur > rarefaction.single(shared=final.opti_mcc.shared, calc=sobs, freq=100)
```


---

## What this process does

Now that we have grouped all our sequences into 495 precise OTUs across our biological samples (`final.opti_mcc.shared`), we must adjust for uneven sequencing depth before we compare them.

Some samples may have 15,000 sequences, while others might only have 2,000. It is biologically unfair to compare the raw diversity of a deeply sequenced sample against a shallowly sequenced one (the deeper one will always look more diverse simply by chance!).

1. **`count.groups`:** This tells us exactly how many sequences are inside each sample. We use this to find the *smallest* sample in our dataset.
2. **`summary.single`:** This calculates **Alpha Diversity** (diversity *within* a single sample). We tell mothur to randomly **subsample** every single group down to the size of the smallest group (in our case, `2404` sequences). This perfectly normalizes the dataset. It then calculates:
   - `sobs`: The observed number of unique species (OTUs).
   - `invsimpson`: The Inverse Simpson diversity index (which accounts for both richness and evenness).
   - `coverage`: Good's coverage (how much of the theoretical entire community we actually sampled).
3. **`rarefaction.single`:** This simulates drawing sequences out of the sample 100 at a time, calculating how many new OTUs are discovered at each step. This allows us to plot Rarefaction Curves to see if our sequencing depth was sufficient to capture the full biodiversity of the environment.

---

## mothur output

### 1. Counting the groups

```
mothur > count.groups(shared=final.opti_mcc.shared)

F3D0 contains 6195.
F3D1 contains 4657.
F3D141 contains 4658.
F3D142 contains 2424.
F3D143 contains 2404.
F3D144 contains 3455.
...
F3D9 contains 5756.

Size of smallest group: 2404.

Total seqs: 114126.
```

Our lowest sequencing depth is **2,404** sequences in sample `F3D143`. We will use this number to normalize the rest of our samples.

### 2. Alpha Diversity with Subsampling

```
mothur > summary.single(shared=final.opti_mcc.shared, calc=nseqs-coverage-sobs-invsimpson, subsample=2404)

Processing group F3D0
0.03
Processing group F3D1
0.03
...
Processing group F3D9
0.03

Output File Names: 
final.opti_mcc.groups.ave-std.summary
```

### 3. Rarefaction Curves

```
mothur > rarefaction.single(shared=final.opti_mcc.shared, calc=sobs, freq=100)

Using 8 processors.
Processing group F3D0
0.03
...

It took 2 secs to run rarefaction.single.

Output File Names: 
final.opti_mcc.groups.rarefaction
```

---

## Output files

Below is a look at the core files generated during this analysis step:

### 1. The Alpha Diversity Summary (`final.opti_mcc.groups.ave-std.summary`)

This file contains the normalized diversity estimates. Because mothur randomly subsamples 1000 times to get a robust average, it outputs both the `ave` (average) and `std` (standard deviation) for each metric.

*Excerpt:*

| label | group | method | nseqs | coverage | sobs | invsimpson | invsimpson_lci | invsimpson_hci |
|---|---|---|---|---|---|---|---|---|
| 0.03 | F3D0 | ave | 2404.000000 | 0.983235 | 146.753000 | 25.769046 | 24.120036 | 27.660186 |
| 0.03 | F3D1 | ave | 2404.000000 | 0.984757 | 143.017000 | 34.818996 | 32.645952 | 37.302092 |
| 0.03 | F3D141 | ave | 2404.000000 | 0.981163 | 134.449000 | 19.594684 | 18.571688 | 20.736985 |
| 0.03 | F3D142 | ave | 2404.000000 | 0.971786 | 156.445000 | 16.865815 | 16.025338 | 17.799333 |
| 0.03 | F3D143 | ave | 2404.000000 | 0.980033 | 140.000000 | 18.673912 | 17.607506 | 19.877821 |

- **`coverage`:** Over ~98% for most samples, meaning our sequencing depth of 2404 captured 98% of the estimated population!
- **`sobs` (Richness):** Shows the normalized number of OTUs in each sample (e.g., F3D0 has ~147 OTUs).
- **`invsimpson` (Diversity):** A higher number means a more diverse and evenly distributed microbial community.

### 2. The Rarefaction Data (`final.opti_mcc.groups.rarefaction`)

This file contains the X/Y coordinate data needed to plot rarefaction curves in R, Excel, or Python.

- X-axis = Number of sequences sampled.
- Y-axis = `F3D0`, `F3D1`, etc. (the number of unique OTUs discovered at that depth).

---

## Next step

The final step for typical alpha/beta diversity analysis inside mothur is to look at **Beta Diversity** (diversity *between* different samples).

We will generate a pairwise distance matrix between all samples using the `dist.shared` command, and potentially run an AMOVA or PCoA to see if different treatment groups cluster separately!
