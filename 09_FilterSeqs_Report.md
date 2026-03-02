# mothur: filter.seqs + unique.seqs (post-filtering)

**Command:**

```
mothur > filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=., processors=4)
```


---

## What this command does

After aligning sequences to the SILVA reference, the resulting alignment contains many columns (features) that are uninformative because they only contain gaps across all our sequences, or because they represent terminal overhangs.

`filter.seqs` removes these uninformative columns to speed up all downstream computations:

- `vertical=T` — removes columns in the alignment that only contain gap characters (often inserted because the full SILVA reference has them, but our V4 sequences do not).
- `trump=.` — removes any column where an overhang character (`.`) occurs in any sequence, strictly constraining the alignment to the overlapping region.

---

## mothur output

```
mothur > filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=., processors=4)

Using 4 processors.
Creating Filter...
It took 8 secs to create filter for 16299 sequences.

Running Filter...
It took 1 secs to filter 16299 sequences.

Length of filtered alignment: 376
Number of columns removed: 13048
Length of the original alignment: 13424
Number of sequences used to construct filter: 16299

Output File Names: 
stability.filter
stability.trim.contigs.good.unique.good.filter.fasta
```

### Filtering results

The alignment was reduced from **13,424 columns** to just **376 columns** (a 97% reduction in size). By stripping away over 13,000 uninformative gap/overhang columns, the dataset becomes much smaller and faster to process, without losing any actual biological sequence information.

---

## Output files

| File | Description |
|------|-------------|
| `stability.filter` | A text-based mask indicating which columns of the original alignment were kept (1) vs removed (0) |
| `stability.trim.contigs.good.unique.good.filter.fasta` | The final trimmed, gap-filtered multiple sequence alignment |

---

---

## Step 2: Re-deduplication (unique.seqs)

**Command:**

```
mothur > unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)
```


---

## What this command does

Because `filter.seqs` removed 13,048 columns of gaps and overhanging sequence ends, some sequences that previously appeared slightly different (due to varying terminal overhangs or gap placements) may now be exactly identical within the compact 376-column V4 region.

We run `unique.seqs` again to collapse these newly identical sequences and further reduce the computational burden.

---

## mothur output

```
mothur > unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)

Output File Names: 
stability.trim.contigs.good.unique.good.filter.unique.fasta
stability.trim.contigs.good.unique.good.filter.count_table
```

### Checking the final alignment (summary.seqs)

```
mothur > summary.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table)

                Start   End     NBases  Ambigs  Polymer NumSeqs
Minimum:        1       376     249     0       3       1
2.5%-tile:      1       376     252     0       3       3217
25%-tile:       1       376     252     0       4       32165
Median:         1       376     252     0       4       64329
75%-tile:       1       376     253     0       5       96493
97.5%-tile:     1       376     253     0       6       125440
Maximum:        1       376     256     0       8       128656
Mean:           1       376     252     0       4
# of unique seqs:       16296
total # of seqs:        128656
```

### Results

The re-deduplication removed **3 redundant unique sequences** (16,299 → 16,296). This is a tiny reduction, indicating that almost all sequences were already unique based on their core V4 region.

Importantly, the summary shows that **all 128,656 sequences now start at position 1 and end at position 376**. The alignment is perfectly flush on both ends.

---

## Output files

| File | Description |
|------|-------------|
| `stability.trim.contigs.good.unique.good.filter.unique.fasta` | The deduplicated, filtered alignment |
| `stability.trim.contigs.good.unique.good.filter.count_table` | Updated count table mapped to the 16,296 unique sequences |

---

## Next step

Pre-cluster the sequences to denoise the data (allowing up to 2 differences/errors):

```
mothur > pre.cluster(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table, diffs=2)
```
