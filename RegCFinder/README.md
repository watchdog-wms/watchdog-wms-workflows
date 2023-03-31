RegCFinder workflow
==================================

Performs:
* Task 1: Computes MSS based on sequencing counts from *.bam files for genomic regions in the regs.bed file
* Task 2: Collects final filtered MSS over all regions and creates annotation file for DEXSeq
* Task 3+4: Computes featureCounts and merges output files into one
* Task 5: Applies DEXSeq on own annotation file and identified MSS
* Task 6: Collects information of DEXSeq and MSS and creates output table with computed scores

---

Required software:
* Watchog (https://www.bio.ifi.lmu.de/software/watchdog/, see below for further installation instructions)
* Conda, e.g. miniconda (https://docs.conda.io/en/latest/miniconda.html)
* All other required software is automatically loaded using conda during the workflow run. This may take some time during the first run or if you delete the conda environments between runs. 

---

How to use RegCFinder:
* Install Watchdog (see below) and Conda.
* Example input files can be downloaded from: https://doi.org/10.5281/zenodo.7585027
* Adapt the Watchdog workflow (= Watchdog_RegCFinder.xml) using a text editor. All places in the xml that have to be adjusted are marked with a TODO comment. 
* Within the constants block, you have to adapt paths to your input files and output directory and can adapt strandness of your experiment, state control and test condition names and set other global parameters. 
* Within the process block, please state a pattern to include your bam files from the bam folder. 
* To receive email updates about your tasks include your email address in the tasks element (if you do not want to get email-updates, just remove mail="mustermann@example.com", a link for progress monitoring will then be printed to standard out). 
* For the amss task, adapt the bam pattern (use the same pattern as for the process block). pseudocount and numrandomizations can also be adjusted in the constants block, but do not have to be. 
* In the the featureCounts Task, adjust the strandness of the experiment. 
* To start RegCFinder, use the following command on the command line: `/path/to/your/watchdog/installation/watchdog.sh -xml /path/to/your/workflow/Watchdog_RegCFinder.xml`

---

Input files:
* regs.bed:  file containing the windows to be analyzed. Consists of one column defining chromosome, start, end and strand, separated by underscore. If the strand is unknown, just use + and specify strandness 0 in the workflow.
* sampleanno.txt: file defining experiment samples. Tab separated file with two columns providing sample name (column "sample") and condition (column "condition"). Samples should be named exactly like the input bam files without file extensions and condition should assign each sample to one of two conditions named as you like. The condition names should also be used in the workflow as test and control. Your input bam files should have a common prefix within each condition and replicates should be marked by a number separated with underscore. E.g.: mock_1, mock_2, WT_1, WT_2.
* directory with indexed bam files 

---

Output:
- Output directories:
   * windows:  directory containing processing results for individual windows from the regs.bed file. Each subdirectory contains the computed and filtered MSS regions and some test figures. 
   * annot: directory containing the annotation files for featureCounts and DEXSeq calls, containing the windows from the input file regs.bed as genes and the identified MSS regions as exons.
   * counts: directory containing the featureCounts output with a summarized count matrix.
   * <test_control>:  directory containing the DEXSeq output created by the DEXSeq module.
   * logfiles: log files containing standard out (.out) and standard error (.err) for individual (sub)tasks
- Output file:
   * scores.tsv: main output of RegCFinder. This tab separated file includes all identified MSS regions together with DEXSeq results. Columns: window (= input window from regs.bed), direction (= which condition dominates), start_seq_idx (= start of MSS), end_seq_idx (= end of MSS), score (= score of MSS), length (= length of MSS), max_x (= position in window where the maximum is observed), max_y (= score at position max_x), padj (=adjusted p-value determined by DEXSeq), log2fold (=log2 fold-change determined by DEXSeq), strand, repx (= MSS score of replicate number x)

---

Install Watchdog:
* Watchdog can be downloaded from: https://github.com/klugem/watchdog
* Installation instructions and Requirements: https://github.com/klugem/watchdog/blob/master/README.md
* Please note that the Watchdog release does not include up-to-date modules. You need to download/install currently available modules from the Watchdog module repository as described in https://github.com/klugem/watchdog/blob/master/README.md.
* If you want to further customize the workflow (e.g. use a computing cluster), the Watchdog manual can be found here: https://www.bio.ifi.lmu.de/files/Software/Watchdog/Watchdog-manual.html


