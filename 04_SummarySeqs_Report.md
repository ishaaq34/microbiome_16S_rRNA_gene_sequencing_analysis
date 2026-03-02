# mothur: summary.seqs

**Command:**

```
mothur > summary.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, processors=4)
```

**Date:** 2026-02-23

---

## What this command does

`summary.seqs` generates a statistical summary of the sequences in a FASTA file. It reports the distribution of sequence lengths, number of ambiguous bases (N), and homopolymer run lengths. This information is used to determine appropriate filtering thresholds for the next step (`screen.seqs`).

The output shows percentiles (2.5%, 25%, median, 75%, 97.5%) so you can see the spread of your data and identify outliers.

---

## mothur output

```
mothur > summary.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, processors=4)

Using 4 processors.

                Start   End     NBases  Ambigs  Polymer NumSeqs
Minimum:        1       248     248     0       3       1
2.5%-tile:      1       252     252     0       3       3810
25%-tile:       1       252     252     0       4       38091
Median:         1       252     252     0       4       76181
75%-tile:       1       253     253     0       5       114271
97.5%-tile:     1       253     253     6       6       148552
Maximum:        1       502     502     249     243     152360
Mean:           1       252     252     0       4
# of unique seqs:       152360
total # of seqs:        152360

It took 7 secs to summarize 152360 sequences.

Output File Names:
stability.trim.contigs.summary
```

---

## Column explanations

| Column | What it means |
|--------|---------------|
| Start | Position where the sequence starts (always 1 for contigs) |
| End | Position where the sequence ends (= sequence length) |
| NBases | Number of bases in the sequence (same as End for contigs) |
| Ambigs | Number of ambiguous bases (N) — positions where R1 and R2 disagreed and both had equal quality, so mothur couldn't resolve the conflict |
| Polymer | Length of the longest homopolymer run (e.g., AAAAAAA = polymer of 7) |
| NumSeqs | Cumulative sequence count at that percentile |

---

## Interpreting the results

**Length (NBases):**

- Median is 252 bp, which is the expected size for 16S V4 amplicons (~253 bp between the V4 primers)
- 97.5% of sequences are 253 bp or shorter — the vast majority are the correct length
- Maximum is 502 bp — these abnormally long sequences are likely non-specific amplification products, chimeras, or read pairs that failed to overlap properly and were concatenated instead of merged. These need to be removed.

**Ambiguous bases (Ambigs):**

- 75% of sequences have 0 ambiguous bases — most contigs assembled cleanly
- 97.5%-tile has 6 ambiguous bases — a small fraction of contigs had positions where R1 and R2 disagreed at equal quality
- Maximum is 249 ambiguous bases — these sequences are essentially noise and must be removed
- For filtering, `maxambig=0` is standard — any contig with ambiguous bases is unreliable

**Homopolymers (Polymer):**

- Median is 4, 97.5%-tile is 6 — most sequences have short homopolymer runs, which is normal
- Maximum is 243 — this is an artifact from the extremely long/erroneous sequences
- For filtering, `maxhomop=8` is standard — the longest biologically real homopolymer in 16S is about 8 bases

---

## What needs to be filtered out

Based on this summary, the following `screen.seqs` thresholds are appropriate (following the MiSeq SOP):

| Parameter | Value | Reason |
|-----------|-------|--------|
| `maxambig` | 0 | Remove any contig with ambiguous bases |
| `maxlength` | 275 | Remove contigs that are too long (non-specific products) |

---

## Output file

| File | Description |
|------|-------------|
| `stability.trim.contigs.summary` | Tab-delimited file with per-sequence statistics (one row per contig) |

---

## Next step

```
mothur > screen.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, maxambig=0, maxlength=275)
```

This will remove sequences with any ambiguous bases or with length greater than 275 bp.
