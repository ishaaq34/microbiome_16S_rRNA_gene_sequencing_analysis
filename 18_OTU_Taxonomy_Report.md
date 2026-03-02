# mothur: OTU Shared Table & Taxonomy

**Commands:**

```
mothur > make.shared(list=final.opti_mcc.list, count=final.count_table, label=0.03)

mothur > classify.otu(list=final.opti_mcc.list, count=final.count_table, taxonomy=final.taxonomy, label=0.03)
```


---

## What this process does

In the previous step, we clustered all our reads into **495 Operational Taxonomic Units (OTUs)**. However, that `.list` file just tells us which sequence ID belongs to which OTU. It didn't tell us:

1. How many sequences of `Otu001` exist in `Sample_A` vs `Sample_B`?
2. What actual biological bacterium is `Otu001`?

We run two final commands to figure this out:

1. **`make.shared`:** This command looks at the massive mapping file and tallies everything up. It creates a matrix (a "shared" file) where the rows are the actual biological sample tubes (e.g., F3D0), the columns are the 495 OTUs, and the numbers inside the matrix are the exact abundance counts. This file is the absolute core requirement for all downstream ecological analysis (Alpha/Beta diversity, ordination, PCoA, etc).
2. **`classify.otu`:** This command looks at every sequence grouped inside an OTU (e.g., all 50,000 sequences inside `Otu001`) and checks the taxonomy we assigned them earlier in the pipeline using SILVA. It then computes a "consensus" taxonomy. If 100% of the sequences in `Otu001` were identified as *Bacteroidetes*, then the consensus taxonomy for `Otu001` becomes *Bacteroidetes*.

---

## mothur output

### 1. Generating the Shared File

```
mothur > make.shared(list=final.opti_mcc.list, count=final.count_table, label=0.03)

0.03

Output File Names:
final.opti_mcc.shared
```

### 2. Classifying the OTUs

```
mothur > classify.otu(list=final.opti_mcc.list, count=final.count_table, taxonomy=final.taxonomy, label=0.03)

0.03

Output File Names: 
final.opti_mcc.0.03.cons.taxonomy
final.opti_mcc.0.03.cons.tax.summary
```

### Interpretation

These commands processed extremely fast and created our final, ultimate matrices needed to answer biological questions!

---

## Output files

| File | Description |
|------|-------------|
| `final.opti_mcc.shared` | The "OTU Table". Rows = Samples. Columns = OTUs. Cells = Number of times that OTU was seen in that sample. |
| `final.opti_mcc.0.03.cons.taxonomy` | A table listing `Otu001`, `Otu002`, etc., and the consensus bacterial lineage assigned to each one. |
| `final.opti_mcc.0.03.cons.tax.summary` | A hierarchical summary of how many total sequences mapped to different taxonomic levels (Phylum, Class, Order, Family, Genus) across the whole project. |

---

## Next step

**CONGRATULATIONS!**
The sequence processing and curation phase of the pipeline is 100% complete.

You now have a clean `final.opti_mcc.shared` count table and a `final.opti_mcc.0.03.cons.taxonomy` file. From this point forward, the pipeline transitions entirely into **Analysis**, which usually involves plotting rarefaction curves, calculating Alpha diversity (Chao1, Shannon), and Beta diversity (AMOVA, PCoA).
