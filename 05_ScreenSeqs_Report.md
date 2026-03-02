# mothur: screen.seqs

**Command:**

```
mothur > screen.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, maxambig=0, maxlength=275, maxhomop=8, processors=4)
```

**Date:** 2026-02-23

---

## What this command does

`screen.seqs` filters out sequences that don't meet quality criteria. Based on the `summary.seqs` results, we set three thresholds:

- `maxambig=0` — remove any contig that has ambiguous bases (N). These are positions where R1 and R2 disagreed at equal quality, so mothur couldn't determine the correct base. Any ambiguity makes the sequence unreliable for taxonomic classification.
- `maxlength=275` — remove contigs longer than 275 bp. The expected V4 amplicon is ~253 bp, so longer sequences are likely non-specific products, chimeras, or failed assemblies where the reads were concatenated instead of merged.
- `maxhomop=8` — remove contigs with homopolymer runs longer than 8 bases. The longest biologically real homopolymer in 16S is about 8, so anything longer is a sequencing error.

---

## mothur output

```
mothur > screen.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table, maxambig=0, maxlength=275, maxhomop=8, processors=4)

Using 4 processors.

It took 7 secs to screen 152360 sequences, removed 23495.

Running command: remove.seqs(accnos=stability.trim.contigs.bad.accnos.temp, count=stability.contigs.count_table)
Removed 23495 sequences from stability.contigs.count_table.

Output File Names:
stability.contigs.pick.count_table

Output File Names:
stability.trim.contigs.good.fasta
stability.trim.contigs.bad.accnos
stability.contigs.good.count_table

It took 8 secs to screen 152360 sequences.
```

---

## Filtering results

| | Count | % of total |
|--|------:|-----------:|
| Input sequences | 152,360 | 100% |
| Removed (bad) | 23,495 | 15.4% |
| Kept (good) | 128,865 | 84.6% |

15.4% of contigs were removed. These are sequences that had at least one ambiguous base, were longer than 275 bp, or had a homopolymer run longer than 8. Losing ~15% at this step is within normal range for 16S amplicon data.

---

## Output files

| File | Description |
|------|-------------|
| `stability.trim.contigs.good.fasta` | Sequences that passed the filter (128,865 contigs) |
| `stability.trim.contigs.bad.accnos` | List of sequence names that were removed |
| `stability.contigs.good.count_table` | Updated count table with only the good sequences |

---

## Note: mothur remembers your settings

mothur is smart enough to remember the correct number of processors, so it will use that throughout your current session unless you change it. To see what mothur knows about your session, run:

```
mothur > get.current()
```

Output:

```
mothur > get.current()

Current RAM usage: 0.0154419 Gigabytes. Total Ram: 16 Gigabytes.

Current files saved by mothur:
processors=10

Current working directory: /Volumes/CrucialX6/18s_rRNA/MiSeq_SOP/

Output File Names:
current_files.summary
```

This shows mothur tracks the current working directory, number of processors, RAM usage, and any files from previous commands. It saves this to `current_files.summary`. This means you don't need to specify `processors=` every time — mothur will reuse the value from your session.

---

## Next step

The next step in the MiSeq SOP is to identify unique sequences to reduce computational load:

```
mothur > unique.seqs(fasta=stability.trim.contigs.good.fasta, count=stability.contigs.good.count_table)
```
