Workflow SRADownload_HISAT2Mapping
==================================

Performs:
* Task 1: Download of HISAT2 index
* Task 2: Extract HISAT2 index from *tar.bz2 file
* Task 3: Downloads Fastq files from SRA
* Task 4: Run HISAT2
* Task 5: Convert SAM to BAM files
* Task 6: Index BAM file

If HISAT2 index is already available on the local file system, skip tasks 1 and 2 (watchdog.sh -x Workflow.SRADownload_HISAT2Mapping.xml -start 3) and update parameter **index** in task 4.

---

Required software:
* GNU wget 
* GNU tar 
* SRA toolkit (download: https://www.ncbi.nlm.nih.gov/sra/docs/toolkitsoft/)
* HISAT2 (download: https://ccb.jhu.edu/software/hisat2/index.shtml)
* samtools (download http://www.htslib.org)
