Workflow Readthrough_Calculation
==================================

##### Performs:
* Task 1: Run featureCounts to obtain read counts per gene on all input bam files
* Task 2: Run samtools flagstat and samtools idxstats on all input bam files
* Task 3: Merge flagstat and idxstats results for input bam files
* Task 4: Calculate downstream transcription, upstream transcription and downstream FPKM

---

##### Required software:
* featureCounts (http://subread.sourceforge.net)
* samtools (https://www.htslib.org)
* Java

##### Ouput
*samplename*_readinreadout.txt (tab-separated)

Important columns:
* Readin_start: start of genomic window for upstream transcription calculation
* Readin_end: end of genomic window for upstream transcription calculation
* Readin_count: number of reads overlapping window for upstream transcription calculation
* Readin: upstream transcription (multiply by 100 for percentage)
* Readout_start: start of genomic window for downstream transcription calculation
* Readout_end: end of genomic window for downstream transcription calculation
* Readout_count: number of reads overlapping window for downstream transcription calculation
* Readout: downstream transcription (multiply by 100 for percentage)
* Downstream_FPKM: FPKM in window for downstream transcription calculation
