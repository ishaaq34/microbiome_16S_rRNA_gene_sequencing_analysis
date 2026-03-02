# mothur: Remove Mock Community & Finalize Dataset

**Commands:**

```
mothur > remove.groups(count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table, fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.fasta, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.silva.wang.pick.taxonomy, groups=Mock)

mothur > rename.file(fasta=current, count=current, taxonomy=current, prefix=final)
```


---

## What this process does

Now that we have fully verified our sequencing error rate and pipeline accuracy using our control Mock community, we no longer need the Mock sequence data intermingling with our actual biological samples!

1. **`remove.groups`:** This strips the `Mock` sample entirely out of our sequence files, count tables, and taxonomy files. This leaves us with a purely biological dataset.
2. **`rename.file`:** Our file names have become absurdly long (e.g., `stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.fasta`). Because this represents the absolute end of the sequence curation phase, we use `rename.file` to copy these final files into clean, simple names (`final.fasta`, etc.) to use for all downstream clustering and ecological analyses.

---

## mothur output

```
mothur > remove.groups(count=stability...count_table, fasta=stability...fasta, taxonomy=stability...taxonomy, groups=Mock)

Removed 4048 sequences from your count file.
Removed 41 sequences from your fasta file.
Removed 41 sequences from your taxonomy file.

Output File names: 
stability...pick.pick.count_table
stability...pick.pick.fasta
stability...pick.pick.taxonomy


mothur > rename.file(fasta=current, count=current, taxonomy=current, prefix=final)
...
Current files saved by mothur:
fasta=final.fasta
taxonomy=final.taxonomy
count=final.count_table
```

### Filtering results

By removing the `Mock` group, we completely dropped its **4,048 sequences** from our count abundance table. Furthermore, 41 unique sequences that *only* existed in the Mock sample were permanently purged from the `.fasta` and `.taxonomy` files to save space.

---

## Output files

We finally have our polished, analysis-ready dataset!

| File | Description |
|------|-------------|
| `final.fasta` | The fully curated, target-only sequences for our biological samples. |
| `final.count_table` | The abundance counts mapping the exact number of times each unique sequence appears in every individual biological sample. |
| `final.taxonomy` | The precise taxonomic lineage assigned to every unique sequence. |

---

## Next step

With sequence curation officially complete, the entire rest of the pipeline is focused on **Analysis**. First, we will group these identical and highly-similar sequences into Operational Taxonomic Units (OTUs) or Amplicon Sequence Variants (ASVs).
