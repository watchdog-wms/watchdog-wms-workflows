RegCFinder workflow
==================================

Performs:
* Task 1: Computes MSS based on sequencing counts from *.bam files for genomic regions in the regs.bed file
* Task 2: Collects final filtered MSS over all regions and creates annotation file for DEXSeq
* Task 3+4: Computes featureCounts and merges output files into one
* Task 5: Apply DEXSeq on own annotation file and identified MSS
* Task 6: Collects information of DEXSeq and MSS and creates output table with computed scores

---

Required software:
* DEXSeq (https://bioconductor.org/packages/release/bioc/html/DEXSeq.html)
* featureCounts (https://subread.sourceforge.net/featureCounts.html)
* GNU R
* Python3
* Java

--- 

A small test example is provided at https://doi.org/10.5281/zenodo.7585027
