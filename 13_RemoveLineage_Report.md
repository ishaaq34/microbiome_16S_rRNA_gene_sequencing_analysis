# mothur: remove.lineage

**Command:**

```
mothur > remove.lineage(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.silva.wang.taxonomy, taxon=Chloroplast-Mitochondria-unknown-Archaea-Eukaryota)
```


---

## What this command does

Even after filtering for sequence quality, proper V4 alignment, and removing chimeras, biological samples will almost always contain unwanted sequences. These typically happen because the 16S primers (despite being designed for Bacteria) will occasionally amplify:

- Eukaryotic 18S RNA (like host DNA or fungi)
- Mitochondria and Chloroplasts (which are evolutionarily derived from bacteria and still share sequence similarity)
- Completely unknown sequences that don't match anything in the database

The `remove.lineage` step looks at the classification data generated in the previous step and explicitly purges any sequence belonging to the unwanted taxonomic lineages defined in the `taxon=` flag.

---

## mothur output

```
mothur > remove.lineage(...)

/******************************************/
Running command: remove.seqs(accnos=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.silva.wang.accnos, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table, fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta)
Removed 10 sequences from stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.fasta.
Removed 49 sequences from stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.count_table.

Output File Names:
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.fasta
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table

/******************************************/

Output File Names:
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.silva.wang.pick.taxonomy
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.silva.wang.accnos
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table
stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.fasta
```

### Lineage removal results

| | Unique seqs | Total seqs |
|--|------:|------:|
| Input | 2,508 | 118,223 |
| Removed | 10 | 49 |
| Output | 2,498 | 118,174 |

This command removed **10 non-target unique sequences**, representing only 49 total reads out of over 118,000.

Because practically all off-target / low-quality material was rigorously removed in the early steps of this pipeline (such as the strict start/end positional `screen.seqs` step and the sequence ambiguity filters), there was barely any eukaryotic/mitochondrial contamination left to remove here.

---

## Output files

| File | Description |
|------|-------------|
| `*.pick.fasta` | The incredibly clean, final fasta file holding only target bacterial sequences. |
| `*.pick.count_table` | The updated sequence counts. |
| `*.silva.wang.pick.taxonomy` | The taxonomy file, now entirely cleared of non-target lineages. |
| `*.silva.wang.accnos` | A list of the sequence IDs that were removed. |

---

## Next step

Our sequences are now fully curated: high quality, properly aligned, denoised, de-replicated, chimera-free, and taxonomy-verified!

Before proceeding to cluster them into OTUs or assessing true community statistics, we need to evaluate the **sequencing error rate** by comparing our sequenced Mock community reads to the *known* true Mock sequences:

```
mothur > seq.error(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table, reference=HMP_MOCK.v35.fasta, aligned=F)
```
