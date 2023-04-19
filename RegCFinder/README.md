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
* Java (for running Watchdog, JDK 11 or higher, https://www.oracle.com/java/technologies/downloads/)
* Watchdog (https://www.bio.ifi.lmu.de/software/watchdog/, see below for further installation instructions)
* Conda, e.g. miniconda (https://docs.conda.io/en/latest/miniconda.html)
* All other required software is automatically loaded using conda during the workflow run. This may take some time during the first run or if you delete the conda environments between runs. 
* RegCFinder has been tested on Linux (SUSE, Release 15.4) and MacOS (Big Sur).

---

How to use RegCFinder:
* Install Watchdog (see below) and Conda.
* Download the workflow xml file from https://raw.githubusercontent.com/watchdog-wms/watchdog-wms-workflows/master/RegCFinder/Watchdog_RegCFinder.xml
* Example input files can be downloaded from: https://doi.org/10.5281/zenodo.7585027
* Adapt the workflow xml file (= Watchdog_RegCFinder.xml) using a text or XML editor. All places in the xml file that have to be adjusted are marked with a TODO comment above the relevant line. 
* To run the example provided at Zenodo, you only have to adjust the installation paths for Watchdog and Conda and the paths to the input and output directory and the directory in which you want so store conda environments for individual tasks.
* To receive email updates about your tasks, change your email address in the tasks element. If you do not want to get email updates or configure email use, just remove mail="mustermann@example.com". In this case, a link for progress monitoring will be printed to standard out.
* In addition, you can adjust the following parts of the workflow for your own input data:
   * Within the constants block, you can adapt strandness of your experiment, state control and test condition names, and adjust pseudocount and numrandomizations for the amss task. 
   * Within the process block, you can adjust the pattern to include selected bam files from the bam folder. 
   * In the amss task, adjust the bam pattern to use the same pattern as for the process block.
   * In the featureCounts Task, adjust the strandness of the experiment. 
* To start RegCFinder, use the following command on the command line: `/path/to/your/watchdog/installation/watchdog.sh -xml /path/to/your/workflow/Watchdog_RegCFinder.xml`

---

Input files:
* regs.bed:  file containing the windows to be analyzed. Consists of one column defining chromosome, start, end and strand, separated by underscore. If the strand is unknown, just use + and specify strandness 0 in the workflow.
* sampleanno.txt: file defining experiment samples. Tab separated file with two columns providing sample name (column "sample") and condition (column "condition"). Samples should be named exactly like the input bam files without file extensions and condition should assign each sample to one of two conditions named as you like. The condition names should also be used in the workflow as test and control. Your input bam files should have a common prefix within each condition and replicates should be marked by a number separated with underscore. E.g.: mock_1, mock_2, WT_1, WT_2.
* directory with indexed bam files 

---

Output:
* Output directories:
   * windows:  directory containing processing results for individual windows from the regs.bed file. Each subdirectory contains the computed and filtered MSS regions and some test figures. 
   * annot: directory containing the annotation files for featureCounts and DEXSeq calls, containing the windows from the input file regs.bed as genes and the identified MSS regions as exons.
   * counts: directory containing the featureCounts output with a summarized count matrix.
   * <test_control>:  directory containing the DEXSeq output created by the DEXSeq module.
   * logfiles: log files containing standard out (.out) and standard error (.err) for individual (sub)tasks
* Output file:
   * scores.tsv: main output of RegCFinder. This tab-separated file includes all identified MSS regions together with DEXSeq results. Columns: window (= input window from regs.bed), direction (= which condition dominates), start_seq_idx (= start of MSS), end_seq_idx (= end of MSS), score (= score of MSS), length (= length of MSS), max_x (= position in window where the maximum is observed), max_y (= score at position max_x), padj (=adjusted p-value determined by DEXSeq), log2fold (=log2 fold-change determined by DEXSeq), strand, repx (= MSS score of replicate number x)

---

Install Watchdog:
* Watchdog can be downloaded from: https://github.com/klugem/watchdog
* Installation instructions and Requirements: https://github.com/klugem/watchdog/blob/master/README.md
* The easiest way to install Watchdog is to download the most recent release from https://github.com/klugem/watchdog/releases and extract the downloaded archive into a folder of your choice.
* Alternatively, you can install Watchdog on Linux using conda by running `conda install bioconda/noarch::watchdog-wms`. In this case, Watchdog can be found in `conda/installation/directory/share/watchdog-wms-2.0.8-0` and the command-line version is run using `watchdog-cmd.sh`. 
* It is *not* necessary to build Watchdog using Maven as required jars are already included in the releases. 
* As macOS comes with the BSD version of getopt, which does not support long flags, a version of the gnu-getop package is shipped with Watchdog. You may get an `App can’t be opened because it is from an unidentified developer` error, when running any Watchdog scripts. You can fix this by doing the following:
    1. Open Finder.
    2. Locate the `/path/to/your/watchdog/installation/core_lib/external/getopt_mac` app.
    3. Control+Click the app.
    4. Select Open.
    5. Click Open.
    6. The app should be saved as an exception in your security settings, allowing you to open it in the future.
* As the Watchdog release does not include up-to-date modules, they have to be obtained from https://github.com/watchdog-wms/watchdog-wms-modules. Either download them directly from github and move them manually into the modules directory or by call `./modules/downloadCommunityModules.sh` from the Watchdog directory.
* If you want to further customize the workflow (e.g. use a computing cluster), the Watchdog manual can be found here: https://www.bio.ifi.lmu.de/files/Software/Watchdog/Watchdog-manual.html


