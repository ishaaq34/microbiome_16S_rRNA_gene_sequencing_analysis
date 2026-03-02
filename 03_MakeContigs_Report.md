# mothur: make.contigs

**Command:**

```
mothur > make.contigs(file=stability.files, processors=4)
```

**Date:** 2026-02-23

---

## What this command does

`make.contigs` takes paired-end reads (R1 and R2) and merges them into a single contiguous sequence (contig). It aligns the overlapping region of R1 and R2, resolves disagreements using quality scores, and outputs the assembled sequence. Reads that cannot be assembled are sent to a scrap file.

The assembly works because Illumina paired-end reads from amplicon sequencing are designed to overlap in the middle:

```
R1 (forward):  5'  =========================>  3'
                         overlap region
R2 (reverse):       3'  <========================  5'
                   |<-------- contig ---------->|
```

When R1 and R2 disagree at a position in the overlap, mothur picks the base with the higher quality score. This is why the low-quality tail of R2 (seen in the FastQC report) is not a major concern — R1 quality is usually higher in that region and gets used instead.

---

## mothur output

Below is the verbose output from `make.contigs`. It shows each file pair being processed and the final group counts:

```
>>>>> Processing file pair F3D0_S188_L001_R1_001.fastq - F3D0_S188_L001_R2_001.fastq (files 1 of 20) <<<<<
Making contigs...
Done.
It took 2 secs to assemble 7793 reads.

>>>>> Processing file pair F3D141_S207_L001_R1_001.fastq - F3D141_S207_L001_R2_001.fastq (files 2 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 5958 reads.

>>>>> Processing file pair F3D142_S208_L001_R1_001.fastq - F3D142_S208_L001_R2_001.fastq (files 3 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 3183 reads.

>>>>> Processing file pair F3D143_S209_L001_R1_001.fastq - F3D143_S209_L001_R2_001.fastq (files 4 of 20) <<<<<
Making contigs...
Done.
It took 0 secs to assemble 3178 reads.

>>>>> Processing file pair F3D144_S210_L001_R1_001.fastq - F3D144_S210_L001_R2_001.fastq (files 5 of 20) <<<<<
Making contigs...
Done.
It took 2 secs to assemble 4827 reads.

>>>>> Processing file pair F3D145_S211_L001_R1_001.fastq - F3D145_S211_L001_R2_001.fastq (files 6 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 7377 reads.

>>>>> Processing file pair F3D146_S212_L001_R1_001.fastq - F3D146_S212_L001_R2_001.fastq (files 7 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 5021 reads.

>>>>> Processing file pair F3D147_S213_L001_R1_001.fastq - F3D147_S213_L001_R2_001.fastq (files 8 of 20) <<<<<
Making contigs...
Done.
It took 4 secs to assemble 17070 reads.

>>>>> Processing file pair F3D148_S214_L001_R1_001.fastq - F3D148_S214_L001_R2_001.fastq (files 9 of 20) <<<<<
Making contigs...
Done.
It took 3 secs to assemble 12405 reads.

>>>>> Processing file pair F3D149_S215_L001_R1_001.fastq - F3D149_S215_L001_R2_001.fastq (files 10 of 20) <<<<<
Making contigs...
Done.
It took 3 secs to assemble 13083 reads.

>>>>> Processing file pair F3D150_S216_L001_R1_001.fastq - F3D150_S216_L001_R2_001.fastq (files 11 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 5509 reads.

>>>>> Processing file pair F3D1_S189_L001_R1_001.fastq - F3D1_S189_L001_R2_001.fastq (files 12 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 5869 reads.

>>>>> Processing file pair F3D2_S190_L001_R1_001.fastq - F3D2_S190_L001_R2_001.fastq (files 13 of 20) <<<<<
Making contigs...
Done.
It took 5 secs to assemble 19620 reads.

>>>>> Processing file pair F3D3_S191_L001_R1_001.fastq - F3D3_S191_L001_R2_001.fastq (files 14 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 6758 reads.

>>>>> Processing file pair F3D5_S193_L001_R1_001.fastq - F3D5_S193_L001_R2_001.fastq (files 15 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 4448 reads.

>>>>> Processing file pair F3D6_S194_L001_R1_001.fastq - F3D6_S194_L001_R2_001.fastq (files 16 of 20) <<<<<
Making contigs...
Done.
It took 2 secs to assemble 7989 reads.

>>>>> Processing file pair F3D7_S195_L001_R1_001.fastq - F3D7_S195_L001_R2_001.fastq (files 17 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 5129 reads.

>>>>> Processing file pair F3D8_S196_L001_R1_001.fastq - F3D8_S196_L001_R2_001.fastq (files 18 of 20) <<<<<
Making contigs...
Done.
It took 2 secs to assemble 5294 reads.

>>>>> Processing file pair F3D9_S197_L001_R1_001.fastq - F3D9_S197_L001_R2_001.fastq (files 19 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 7070 reads.

>>>>> Processing file pair Mock_S280_L001_R1_001.fastq - Mock_S280_L001_R2_001.fastq (files 20 of 20) <<<<<
Making contigs...
Done.
It took 1 secs to assemble 4779 reads.


Group count:
F3D0    7793
F3D1    5869
F3D141  5958
F3D142  3183
F3D143  3178
F3D144  4827
F3D145  7377
F3D146  5021
F3D147  17070
F3D148  12405
F3D149  13083
F3D150  5509
F3D2    19620
F3D3    6758
F3D5    4448
F3D6    7989
F3D7    5129
F3D8    5294
F3D9    7070
Mock    4779

Total of all groups is 152360

It took 37 secs to process 152360 sequences.

Output File Names:
stability.trim.contigs.fasta
stability.scrap.contigs.fasta
stability.contigs_report
stability.contigs.count_table
```

All 152,360 read pairs were successfully assembled with zero failures. Read depth varies across samples (3,178 to 19,620), which is normal for multiplexed MiSeq runs. Downstream analysis steps (rarefaction or normalization) handle this variation.

---

## Output files

| File | Description |
|------|-------------|
| `stability.trim.contigs.fasta` | Assembled contigs — 152,360 sequences ready for downstream processing |
| `stability.scrap.contigs.fasta` | Failed assemblies — empty (0 sequences). All read pairs assembled successfully. |
| `stability.contigs_report` | Per-contig statistics: length, overlap length, overlap coordinates, number of mismatches, N count, and expected errors |
| `stability.contigs.count_table` | Count table tracking how many times each unique sequence appears in each sample group |

---

## Contigs report format

The `stability.contigs_report` file contains one line per contig with the following columns:

| Column | Description |
|--------|-------------|
| Name | Sequence identifier from the original FASTQ header |
| Length | Length of the assembled contig (bp) |
| Overlap_Length | Number of bases where R1 and R2 overlapped |
| Overlap_Start | Start position of the overlap in the contig |
| Overlap_End | End position of the overlap in the contig |
| MisMatches | Number of positions where R1 and R2 disagreed in the overlap |
| Num_Ns | Number of ambiguous bases (N) in the contig |
| Expected_Errors | Sum of error probabilities across the contig (derived from quality scores) |

Raw output from `head -5 stability.contigs_report`:

```
Name Length Overlap_Length Overlap_Start Overlap_End MisMatches Num_Ns Expected_Errors
M00967_43_000000000-A3JHG_1_1101_18327_1699 252 250 1 251 2 1 0.637243
M00967_43_000000000-A3JHG_1_1113_11294_24024 252 250 1 251 1 0 0.00437234
M00967_43_000000000-A3JHG_1_1108_9640_17327 252 250 1 251 7 0 0.0240291
M00967_43_000000000-A3JHG_1_2107_18412_10286 252 249 2 251 1 0 0.00414817
```

Breaking down the first contig for readability:

```
Name:             M00967_43_000000000-A3JHG_1_1101_18327_1699
Length:           252
Overlap_Length:   250
Overlap_Start:    1
Overlap_End:      251
MisMatches:       2
Num_Ns:           1
Expected_Errors:  0.637243
```

---

## Next step

The next step in the mothur MiSeq SOP is to summarize the contigs and then filter by length and ambiguous bases:

```
mothur > summary.seqs(fasta=stability.trim.contigs.fasta, count=stability.contigs.count_table)
```

This will report the distribution of contig lengths, number of ambiguous bases, and homopolymer lengths, which helps determine appropriate filtering thresholds for `screen.seqs`.
