Workflow RNA_DifferentialGeneExpression
==================================

Performs:
* Task 1: Unzip FASTQ reads from *.fastq.gz files
* Task 2: Assess FASTQ quality using FastQC
* Task 3: Align reads using ContextMap in combination with bwa
* Task 4+5: Convert SAM to BAM format and index it
* Task 6: Create statistics on BAM files using RSeQC
* Task 7+8: Run featureCounts to obtain read counts per gene
* Task 9+10+11+12: Merge collected statistics of all samples
* Task 13: Create contigt distribution plot of mapped reads
* Task 14: Merge featureCounts output
* Task 15: Detect differentially expressed genes using DESeq2
---

Required software:
* FastQC (https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* bwa (http://bio-bwa.sourceforge.net)
* ContextMap (https://www.bio.ifi.lmu.de/software/contextmap/)
* samtools (https://www.htslib.org)
* RSeQC (https://rseqc.sourceforge.net)
* featureCounts (https://subread.sourceforge.net)
* DESeq2 (https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
* GNU R
* Python3
* Java

