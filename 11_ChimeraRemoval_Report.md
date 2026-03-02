# mothur: chimera.vsearch

**Command:**

```
mothur > chimera.vsearch(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t, processors=4)
```


---

## What this command does

During PCR amplification, incomplete extension sequences can act as primers on different template strands during subsequent cycles. This creates *chimeras* — artificial DNA sequences that are fusions of two biological sequences.

The MiSeq SOP uses the VSEARCH algorithm (an open-source alternative to USEARCH) to identify these artifacts:

- It checks each sequence against more abundant sequences in the same sample.
- If a sequence appears to be a hybrid of two more abundant sequences, it is flagged as chimeric.
- `dereplicate=t` tells the algorithm to remove the chimera if it is found in any sample, even if it might appear non-chimeric in another sample.

---

## mothur output

```
mothur > chimera.vsearch(...)

...
Taking abundance information into account, this corresponds to
X chimeras, Y non-chimeras,
and Z borderline sequences in total sequences.
...

Removing chimeras from your input files:
/******************************************/
Running command: remove.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, accnos=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.accnos)
Removed 3582 sequences from stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta.

Output File Names:
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.chimeras
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.accnos
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta
```

### Checking the results (summary.seqs)

```
mothur > summary.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table)

                Start   End     NBases  Ambigs  Polymer NumSeqs
Minimum:        1       376     249     0       3       1
2.5%-tile:      1       376     252     0       3       2956
25%-tile:       1       376     252     0       4       29556
Median:         1       376     252     0       4       59112
75%-tile:       1       376     253     0       5       88668
97.5%-tile:     1       376     253     0       6       115268
Maximum:        1       376     256     0       8       118223
Mean:           1       376     252     0       4
# of unique seqs:       2508
total # of seqs:        118223
```

### Chimera removal results

| | Unique seqs | Total seqs |
|--|------:|------:|
| Input | 6,090 | 128,656 |
| Removed | 3,582 | 10,433 |
| Output | 2,508 | 118,223 |

Chimera checking removed over half of our remaining unique sequences (3,582 out of 6,090 unique seqs removed).
However, because chimeras are generally rare in abundance compared to true sequences, we only lost **10,433 total sequences** (about **8%** of the total reads).

We are left with **2,508 high-quality, non-chimeric unique sequences** (representing a total dataset of 118,223 reads).

---

## Output files

| File | Description |
|------|-------------|
| `*.denovo.vsearch.fasta` | The final clean, denoised, and non-chimeric alignment |
| `*.denovo.vsearch.count_table` | Updated sequence count footprint dropping all chimeras |
| `*.denovo.vsearch.chimeras` | Detailed file showing exactly which sequences were identified as chimeras |
| `*.denovo.vsearch.accnos` | A simple text list containing the names (accessions) of the chimeric sequences |

---

## Next step

The final sequence curation step before clustering into OTUs or ASVs is removing sequences that classify to undesirable lineages (like Chloroplasts, Mitochondria, Archaea, or Eukaryota, depending on the study goals).

First, we must classify the sequences against the reference database:

```
mothur > classify.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table, reference=silva.nr_v138_1.align, taxonomy=silva.nr_v138_1.tax, cutoff=80)
```
