# mothur: make.file

**Command:**

```
mothur > make.file(inputdir=., type=fastq, prefix=stability)
```

**Date:** 2026-02-23

---

## What this command does

`make.file` scans the input directory for FASTQ files and automatically pairs forward (R1) and reverse (R2) reads based on their filenames. It creates a tab-delimited file (`stability.files`) that mothur uses in subsequent commands, particularly `make.contigs`.

**Parameters used:**

| Parameter | Value | Meaning |
|-----------|-------|---------|
| `inputdir` | `.` | Look for FASTQ files in the current directory (MiSeq_SOP) |
| `type` | `fastq` | Only look for `.fastq` files (not `.fasta` or `.gz`) |
| `prefix` | `stability` | Name the output file `stability.files` |

---

## Output

**Output file:** `MiSeq_SOP/stability.files`

The file contains 20 sample entries with three tab-separated columns:

| Column | Description |
|--------|-------------|
| 1 | Sample group name (derived from the filename prefix) |
| 2 | Forward read file (R1) |
| 3 | Reverse read file (R2) |

### Contents of stability.files

```
F3D0 F3D0_S188_L001_R1_001.fastq F3D0_S188_L001_R2_001.fastq
F3D141 F3D141_S207_L001_R1_001.fastq F3D141_S207_L001_R2_001.fastq
F3D142 F3D142_S208_L001_R1_001.fastq F3D142_S208_L001_R2_001.fastq
F3D143 F3D143_S209_L001_R1_001.fastq F3D143_S209_L001_R2_001.fastq
F3D144 F3D144_S210_L001_R1_001.fastq F3D144_S210_L001_R2_001.fastq
F3D145 F3D145_S211_L001_R1_001.fastq F3D145_S211_L001_R2_001.fastq
F3D146 F3D146_S212_L001_R1_001.fastq F3D146_S212_L001_R2_001.fastq
F3D147 F3D147_S213_L001_R1_001.fastq F3D147_S213_L001_R2_001.fastq
F3D148 F3D148_S214_L001_R1_001.fastq F3D148_S214_L001_R2_001.fastq
F3D149 F3D149_S215_L001_R1_001.fastq F3D149_S215_L001_R2_001.fastq
F3D150 F3D150_S216_L001_R1_001.fastq F3D150_S216_L001_R2_001.fastq
F3D1 F3D1_S189_L001_R1_001.fastq F3D1_S189_L001_R2_001.fastq
F3D2 F3D2_S190_L001_R1_001.fastq F3D2_S190_L001_R2_001.fastq
F3D3 F3D3_S191_L001_R1_001.fastq F3D3_S191_L001_R2_001.fastq
F3D5 F3D5_S193_L001_R1_001.fastq F3D5_S193_L001_R2_001.fastq
F3D6 F3D6_S194_L001_R1_001.fastq F3D6_S194_L001_R2_001.fastq
F3D7 F3D7_S195_L001_R1_001.fastq F3D7_S195_L001_R2_001.fastq
F3D8 F3D8_S196_L001_R1_001.fastq F3D8_S196_L001_R2_001.fastq
F3D9 F3D9_S197_L001_R1_001.fastq F3D9_S197_L001_R2_001.fastq
Mock Mock_S280_L001_R1_001.fastq Mock_S280_L001_R2_001.fastq
```

**Total: 20 samples** (19 biological samples from the mouse gut timecourse + 1 Mock community control)

---

## How mothur pairs the files

mothur uses the Illumina filename convention to match R1 and R2:

```
{SampleName}_{SampleNumber}_{Lane}_{Read}_{Set}.fastq
```

For example:

- `F3D0_S188_L001_R1_001.fastq` → forward read
- `F3D0_S188_L001_R2_001.fastq` → reverse read

The group name (first column) is extracted from the portion before the first underscore (`F3D0`, `F3D141`, `Mock`, etc.).

---

## Sample naming

The sample names follow the MiSeq SOP convention:

- **F3D0** through **F3D9**: Early timepoints (Day 0 through Day 9 post-weaning)
- **F3D141** through **F3D150**: Late timepoints (Day 141 through Day 150 post-weaning)
- **Mock**: Mock community control containing known bacterial species (used to assess sequencing error rates)

Note: F3D4 is missing from the dataset — this is intentional in the tutorial data.

---

## Next step

This file is used as input for `make.contigs`, which merges R1 and R2 reads into contiguous sequences:

```
mothur > make.contigs(file=stability.files, processors=4)
```
