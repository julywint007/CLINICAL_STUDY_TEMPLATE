# Template Reporting Effort repository

This repo and corresponding Domino Project contains the folder structure to template a reporting effort making ADaM and TFL code for the Domino clinical trial demo.

This repo project is copied by Clinical Programming team to instantiate a new study specfic reporting effort GitHub repo and Domino Project.


# Directory structure

The programming is created in a typical clinical trial folder structure, where the production (prod) and qc programs have independent directory trees.

Reporting effort level standard code (e.g. SAS macros) should be stored in the `share/macros` folder.

The global `domino.sas` autoexec progam is also included in the repository to appropriately set up the SAS environment. 

```
repo
│   domino.sas
├───prod
│   ├───adam
│   └───tfl
├───qc
│   ├───adam
│   │       compare_adam.sas
│   └───tfl
├───utilities
│       init_datasets_re.py
│       import_metadata.sas
├───pipelines
│       snakemake.sh
│       Snakefile
│       multijob.py
│       jobs_example.cfg
└───share
    └───macros
```

# Setup

1. Create a new project, named `CDISC01_RE_YOURNAME`, from the project template. This will create a new project and a new study specfic GitHub repo.
1. Run `utilities/dataset_init_re.py` as a job to create the appropriate analysis domino datasets (ADAM, TFL, ADAMQC, TFLQC and METADATA). As well as import SDTM datasets from an existing project following the same naming convention (`CDISC01_SDTM` for `CDISC01_RE_XXXXX`).
1. Add the external data volume (EDV) `metadata-repository` to your project.

     a. Ask your Domino contact on how to set up this example EDV within your Domino deployment. 
1. Set the `DCUTDTC` project environment variable to the same value as the latest SDTM Snapshot e.g. MAR122024.
2. Set the `SDTM_DATASET` project environment variable to `SDTMBLIND`.
1. Run `utilities/import_metadata.sas` as a job (on the SAS environment!) to move and transform the metadata Excel file stored in the `metadata-repository` EDV to sas7bdat files in your local METADATA project dataset.
1. Run each of your prod ADaM and TFL programs in the Jobs view to produce your outputs.

# Naming convention

The programs follow a typical clinical trial naming convention, where the ADaM programs are named using the dataset name (e.g. ADSL.sas, etc.) and the TFL programs have a `t_` prefix to indicate tables, etc.

# QC programming and reporting

The QC programming is all in SAS, and there is a `compare.sas` program which uses SAS PROC COMPARE to create a summary report of all differences between the prod and qc datasets. This program also generates the `dominostats.json` files which Domino uses to display a dashboard in the jobs screen.

`compare.sas` references a read-only SAS macro stored in the [`SCE_STANDARD_LIB`](https://github.com/dominodatalab/SCE_STANDARD_LIB) repo so ensure this is imported as a secondary repo in order to run it.

# Support

Programming was created by Veramed Ltd. on behalf of Domino Data Lab, Inc.
