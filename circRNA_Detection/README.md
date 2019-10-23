Workflow circRNA_Detection
==================================

Performs:
* Task 1: Run CIRI2
* Task 2+3: Copy CIRI2 output files to final output directory
* Task 4: Run circRNA_finder
* Task 5+6: Copy circRNA_finder output files to final output directory
* Task 7: Combine results from CIRI2 and circRNA_finder
* Task 8: Copy combined results (*intersectedUnion.txt, contains all circRNAs found by both tools, with reads found by at least one) to final output directory
* Task 9: Remove reads can be mapped in a linear fashion by the used RNA-seq mapping tool (e.g. HISAT2), remove circRNAs without any reads remaining
* Task 10: Copy filtered results final output directory, delete large tempory files


---

Required software:
* CIRI2 (download: https://sourceforge.net/projects/ciri/files/CIRI2/)
* bwa (download: http://bio-bwa.sourceforge.net)
* circRNA_finder (download: https://github.com/orzechoj/circRNA_finder)
* STAR (download: https://github.com/alexdobin/STAR)
* samtools (download http://www.htslib.org)

