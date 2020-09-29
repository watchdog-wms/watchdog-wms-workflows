Workflow ChIP_PeakDetection
==================================

Performs:
* Task 1: Unzip FASTQ reads from *.fastq.gz files
* Task 2: Assess FASTQ quality using FastQC
* Task 3+4: Align reads using bwa
* Task 5: Use bwa sampe to pair reads in alignment
* Task 6: Filter reads based on MAPQ quality and remove unpaired reads
* Task 7+8: Convert SAM to BAM format and index it
* Task 9: Run peak detection using GEM in GPS mode
* Task 10+11: Create some basic plots and annotate the peaks using ChIPseeker
* Task 12: Delete large tempory files

---

Required software:
* FastQC (https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* bwa (http://bio-bwa.sourceforge.net)
* GEM (https://groups.csail.mit.edu/cgs/gem/)
* ChIPseeker (https://www.bioconductor.org/packages/release/bioc/html/ChIPseeker.html)
* samtools (https://www.htslib.org)
* GNU R
* Python3
* Java
