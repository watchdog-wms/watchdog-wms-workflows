Workflow dOC_calculation
==================================

##### Performs:
* Task 1: Run F-Seq to determine open chromatin regions (OCRs) on all input bam files
* Task 2: Calculate dOCR lengths from OCRs determined in Task 1
* Task 3: Run samtools flagstat and samtools idxstats on all input bam files
* Task 4: Merge flagstat and idxstats results for input bam files
* Task 5: Determine downsampling rates for each sample such that all downsampled files contain approximately the same number of reads mapping to the host genome
* Task 6: Perform downsampling
* Task 7: Run F-Seq to determine open chromatin regions (OCRs) on downsampled bam files
* Task 8: Calculate dOCR lengths from OCRs determined in Task 7

---

##### Required software:
* samtools (https://www.htslib.org)
* picard (https://broadinstitute.github.io/picard/)
* Java
* Python3

---

##### Output
dOCR lengths are contained in file *samplename*_combined_0_0.txt (tab-separated) in column downstream_length.

--- 
### Example input files
BAM files for ATAC-seq data for mock and HSV-1 strain 17 infection and infection with null mutants of HSV-1 can be downloaded from https://doi.org/10.5281/zenodo.7766550
