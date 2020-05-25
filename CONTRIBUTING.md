### Contributing

Contributions to the [watchdog-wms-workflows](https://github.com/watchdog-wms/watchdog-wms-workflows) repository are welcome!
If you want to share a workflow, please follow these steps:

1) fork the repository and create a new branch for your new workflow
2) commit your workflow into your fork
    - either use the webinterface of Github (buttons are located left of green "Clone or Download" button)
        - Create new file: create and commit a single file in a web editor
        - Upload Files Button: use drag & drop gestures to upload multiple files or complete folders
    - or clone the repository and use the git command line tools to create and push a new commit
3) create a pull request
4) automatic tests will be performed on your pull request (by Travis CI)
5) members of the contributor team can merge your pull request if all checks succeed

More detailed instructions how to contribute to a github repository can be found [here](https://github.com/firstcontributions/first-contributions).


### Join the contributor team
Members of the contributor team have write access to the repository and can merge pull requests if all Travis CI checks succeeded.

To become a member, please follow these steps:
1) share a few workflows
2) request to become a member by mentioning watchdog-wms-bot in one of your pull requests (just include '@watchdog-wms-bot' in your comment)

### Workflow structure

Workflows shared in the repository have to located in separate directories. Each workflow directory has to contain the XML workflow file (*workflowName.xml*), a README.md ([README template](https://github.com/watchdog-wms/watchdog-wms-workflows/blob/master/README.template.md)) file and may optionally contain example data. Workflows should be documented with inline comments. Furthermore, lines that require modifications to adapt e.g. to different computing environments or input data should be highlighted in order to allow everyone to quickly adapt the workflow.

Example:

    SRADownload_HISAT2Mapping
    ├── README.md
    ├── SRA_sample_file.txt
    └── Workflow.SRADownload_HISAT2Mapping.xml
  
More information on how to create a workflow can be found in the [documentation](https://rawgit.com/klugem/watchdog/master/documentation/Watchdog-manual.html).
   
### General workflow design guidelines

Try to design your workflow as reusable as possible. The following points can be used as guideline:

1) Define constants and use them in the workflow to set paths or other important parameters (e.g. ${PATH2XYZ}).
2) Define one or more base directories as constants and define all absolute paths in the workflow relative to them (e.g. ${BASEDIR}/test1/file1.fasta).
3) Use process blocks and file patterns to process replicates (e.g. <processFolder ... pattern="*_R[12].fastq.gz" />) .
4) Use {}/()/[] to reference the absolute path to/absolute path to the parent folder of/name of the input file for a subtask created via a processFolder process block.
5) Removing dependencies after running a workflow may lead to inconsistencies in re-runs and thus should always be avoided. Adding additional dependencies after running a workflow does not constitute a problem. 

### Automatic tests on pull requests

Before a pull request can be merged with the master branch some Travis CI tests must succeed.
Currently the following tests are implemented:

- signed commit test: 
  - only [signed commits](https://help.github.com/en/articles/signing-commits) can be merged 
- separate folder test: 
  - all files affected by the pull request must be located in one folder (which must be a child of the root folder)
- XML validation test:
  - XML workflow file must follow its [XSD schema](https://github.com/klugem/watchdog/blob/master/xsd/watchdog.xsd)
  - all used modules must be part of [watchdog-wms-modules](https://github.com/watchdog-wms/watchdog-wms-modules)
  - if a process table is used, the path must point to ${WF_PARENT}/nameOfTheFile.csv to be found
- README existence test:
  - test if the README.md file exists (file should be based on the [README template](https://github.com/watchdog-wms/watchdog-wms-workflows/blob/master/README.template.md))
- virus scanner test: 
  - all files part of the pull request must pass the virus scan

