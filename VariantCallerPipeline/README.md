VariantCallerPipeline workflow
==================================

Performs:
* Task 1: Calls SNPs with bcftools
* Task 2: Calls SNPs with VarScan
* Task 3: Computes consistent SNPs
* Task 4: Predicts the virus strain
* Task 5: Matches SNPs to genomic features
* Task 6: Extracts genome coverage into bedgraph file
* Task 7: Extracts clipped reads into BAM file
* Task 8: Calls deletions and insertions
* Task 9: Matches deletions to genomic features
* Task 10: Matches insertions to genomic features
* Task 11: Performs a sequence assembly using SPades
* Task 12: Builds an index for the assembly using BWA
* Task 13: Searches consensus sequences in the index
* Task 14: Extracts insertion sequences

---

Required software:
* Java (for running Watchdog, JDK 11 or higher, https://www.oracle.com/java/technologies/downloads/)
* Watchdog (https://www.bio.ifi.lmu.de/software/watchdog/, see below for further installation instructions)
* Conda, e.g. miniforge (https://github.com/conda-forge/miniforge)
* All other required software is automatically loaded using conda during the workflow run. This takes some time when you run the VariantCallerPipeline workflow for the first time or if you delete the conda environments between runs.

---

How to use the VariantCallerPipeline:
* Install Watchdog (see below) and Conda.
* Download the workflow xml file from https://raw.githubusercontent.com/watchdog-wms/watchdog-wms-workflows/master/VariantCallerPipeline/Watchdog_VariantCallerPipeline.xml
* Adapt the workflow xml file (= Watchdog_VariantCallerPipeline.xml) using a text or XML editor. All places in the xml file that have to be adjusted are marked with a TODO comment above the relevant line. 
* To run the workflow, you only have to adjust the installation paths for Watchdog and Conda and the paths to the required directories and files and the directory in which you want to store conda environments for individual tasks.
* Example input files can be downloaded from https://doi.org/10.5281/zenodo.14266852.
* To receive email updates about your tasks, change your email address in the tasks element. If you do not want to get email updates or configure email use, just remove mail="mustermann@example.com". In this case, a link for progress monitoring will be printed to standard out.
* You can adjust the constants block of the workflow to specify the following parameters:
   * The base directory, in which not only the outputs/results but also log files will be stored.
   * Paths to your input files, including the reference genome, reference SNPs, a config file with sample-strain matches, a GTF file and the paths to your aligned sequencing data in BAM format
* To start the VariantCallerPipeline, use the following command on the command line: `/path/to/your/watchdog/installation/watchdog.sh -xml /path/to/your/workflow/Watchdog_VariantCallerPipeline.xml`

---

Input files:
* `reference_genome.fa`: fasta file containing the reference genome, on which the input sequencing data was aligned to.
* `reference_snps.txt`: tab separated file containing reference SNPs and their corresponding reference sample, of which the virus strain is already known. This is used for strain prediction. The first column corresponds to the chromosome the SNP is located on, the second to the reference sample the SNP was found in and the third to the exact genomic position. The file has **no** header line:

|   |   |   |
|---|---|---|
| chr_X | ref_sample_A | SNP_position |
| chr_X | ref_sample_B | SNP_position |
| . . . | . . . | . . . |

* `config_strain.txt`: tab separated file containing reference samples and their corresponding virus strain. Note: both `reference_snps.txt` and `config_strain.txt` are already available for HSV-1 strains 17, F and KOS and can be found in the directory of the `identifyStrain` module of the workflow. If you like to predict other strains of the same or also of a different virus, please create these files on your own with samples with already known virus strains. The first column corresponds to the name of the reference sample, the second to its corresponding virus strain:

| **SAMPLE**  |  **STRAIN** |
|---|---|
| ref_sample_A | strain_1 |
| ref_sample_B | strain_2 |
| . . . | . . . |

* `annotation.gtf`: tab separated file in the gene transfer format containing annotation of the corresponding viral genome/features.
* `samples.txt`: tab separated file containing the paths to all replicates of the input data. It is in a specific format, so that each row corresponds to one **sample**. Note: If a sample has less replicates than the sample with the most replicates, fill `NA` in the remaining fields for the replicates instead of a file path.

| **SAMPLE**         | **BAM_1**               | **BAM_2**       | **. . .**       | **BAM_X**       |
|---------------------|--------------------------------|---------------------|--------------------|--------------------|
| sample_A             | /path/to/replicate_1               | /path/to/replicate_2              | . . .     | /path/to/replicate_X |
| sample_B            | /path/to/replicate_1    | /path/to/replicate_2 | . . .                 | /path/to/replicate_X |
| . . .            | . . .      | . . .               | . . .    | . . . |
* `replicates.txt`: file containing the paths to all replicates of the input data. It is in a specific format, so that each row corresponds to one **replicate**:

| **SAMPLE**         | **BAM**               | **FASTQ_R1**       | **FASTQ_R2**       |
|---------------------|--------------------------------|---------------------|--------------------|
| sample_A             | /path/to/replicate_1               | /path/to/forward/reads/replicate_1    | /path/to/reverse/reads/replicate_1     |
| sample_A            | /path/to/replicate_2     | /path/to/forward/reads/replicate_2 | /path/to/forward/reads/replicate_2            |
| . . .            | . . .     | . . . | . . .            |
| sample_A            | /path/to/replicate_X     | /path/to/forward/reads/replicate_X | /path/to/forward/reads/replicate_X            |
| sample_B             | /path/to/replicate_1               | /path/to/forward/reads/replicate_1      | /path/to/reverse/reads/replicate_1     |
| . . .            | . . .      | . . .               | . . .    |


---

Output:

There are two main output directories:
* `logs`: directory, in which log files containing standard out (.out) and standard error (.err) for individual (sub)tasks are stored.
* `results`: directory, in which files that contain analysis results from the pipeline are stored.
   * `bcftools`: directory containing output files from bcftools in the variant call format.
   * `varscan`: directory containing output files from VarScan in a (comparable) variant call format.
   * `consistent`: directory containing files with consistent SNPs for each sample.
   * `strains`: directory containing files with virus strain predictions for each sample.
   * `gtfMatcher_SNPs`: directory containing files with information about features (e.g. genes) that matched to consistent SNPs.
   * `genomeCoverage`: directory containing bedgraph files.
   * `clippedReads`: directory containing BAM files with only clipped reads.
   * `deletions`: directory containing files with detected deletions.
   * `insertions`: directory containing files with detected insertions.
   * `consensus`: directory containing files with consensus sequences of insertion start and end.
   * `gtfMatcher_deletions`: directory containing files with information about features (e.g. genes) that matched to detected deletions.
   * `gtfMatcher_insertions`: directory containing files with information about features (e.g. genes) that matched to detected insertions.
   * `SPades`: directory containing output directories for the assemblies by SPades.
   * `bwaMEM`: directory containing SAM files that store information about the alignment of consensus sequences to the assembly.
   * `assemblyAnalyzer`: directory containing fasta files with extracted insertion sequences.

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


