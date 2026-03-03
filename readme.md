# 18S rRNA Pipeline Index

This directory contains a complete, step-by-step bioinformatics pipeline for processing 16S/18S rRNA sequence data through the **mothur** software package. 

> [!NOTE]
> This pipeline and the accompanying reports are heavily inspired by the official [mothur MiSeq SOP](https://mothur.org/wiki/miseq_sop/). 

The pipeline begins with raw Illumina paired-end FASTQ sequences and moves through four primary phases:
1. **Quality Control & Assembly (Steps 1-4):** Merging paired-end reads and gathering baseline statistics.
2. **Alignment & Curation (Steps 5-16):** Aligning sequences to a reference database (e.g., SILVA), removing uninformative columns, filtering out sequencing errors (via `pre.cluster`), and removing chimeric artifacts (via `vsearch`).
3. **Classification & Clustering (Steps 17-18):** Assigning taxonomy to the curated reads and clustering them into Operational Taxonomic Units (OTUs).
4. **Statistical Analysis (Steps 19-21):** Normalizing the data to calculate Alpha Diversity (within-sample richness), Beta Diversity (between-sample variation via PCoA/AMOVA), and identifying specific differential biomarkers between treatment groups using LEfSe.

1. [FastQC Quality Control Report](01_FastQC_Report_F3D0.md)
2. [mothur: make.file](02_MakeFile_Report.md)
3. [mothur: make.contigs](03_MakeContigs_Report.md)
4. [mothur: summary.seqs](04_SummarySeqs_Report.md)
5. [mothur: screen.seqs](05_ScreenSeqs_Report.md)
6. [mothur: unique.seqs](06_UniqueSeqs_Report.md)
7. [mothur: summary.seqs (post-filtering)](07_SummarySeqs2_Report.md)
8. [mothur: pcr.seqs + align.seqs](08_AlignSeqs_Report.md)
9. [mothur: filter.seqs + unique.seqs (post-filtering)](09_FilterSeqs_Report.md)
10. [mothur: pre.cluster](10_PreCluster_Report.md)
11. [mothur: chimera.vsearch](11_ChimeraRemoval_Report.md)
12. [mothur: classify.seqs](12_ClassifySeqs_Report.md)
13. [mothur: remove.lineage](13_RemoveLineage_Report.md)
14. [mothur: seq.error](14_SeqError_Report.md)
16. [mothur: Remove Mock Community & Finalize Dataset](16_FinalCuration_Report.md)
17. [mothur: OTU Clustering](17_OTUClustering_Report.md)
18. [mothur: OTU Shared Table & Taxonomy](18_OTU_Taxonomy_Report.md)
19. [mothur: Rarefaction & Alpha Diversity](19_Rarefaction_AlphaDiversity_Report.md)
20. [mothur: Beta Diversity](20_BetaDiversity_Report.md)
21. [mothur: Differential Abundance & Biomarker Discovery](21_DifferentialAbundance_Report.md)
