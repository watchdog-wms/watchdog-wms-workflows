Workflow ChIP_PeakDetection
==================================

Performs:
* Task 1: Unzip FASTQ reads from *.fastq.gz files
* Task 2+3: Align reads using bwa
* Task 4: Use bwa sampe to pair reads in alignment
* Task 5: Filter reads based on MAPQ quality and remove unpaired reads
* Task 6+7: Convert SAM to BAM format and index it
* Task 8: Run peak detection using GEM in GPS mode
* Task 9+10: Create some basic plots and annotate the peaks using ChIPseeker
* Task 11: Delete large tempory files

---

Required software:
* bwa (http://bio-bwa.sourceforge.net)
* GEM (https://groups.csail.mit.edu/cgs/gem/)
* ChIPseeker (https://www.bioconductor.org/packages/release/bioc/html/ChIPseeker.html)
* samtools (https://www.htslib.org)
* GNU R
* Python3
* Java
