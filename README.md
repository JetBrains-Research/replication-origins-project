# replication-fork-project
IB 2021 Spring Student Project

# Goal of the project
The aim of the project is to investigate methylation patterns changes in replication origins regions in people of different age groups.

# Plan
The main objectives of the project are: 

  * create a Snakemake pipeline to get consistent data 
  * develop a method for determining conservative peaks
  * select methylation datasets for different age groups
  * correlate methylation data with replication origins regions
  * assess methylation signal changes and methylation variability

# Requirements
Python >= 3.6

Packages: all requirement packages are in `environment.yml`. Install them with `conda env create -f environment.yml`.

# Instructions

## Run pipeline:

### Prerequisites

Install mamba package manager (recommended for Snakemake)
```shell
conda activate base
conda install -c conda-forge mamba
```

Configure environment:
```shell
conda env create -f environment.yml -p .conda_env 
conda activate "$(pwd)/.conda_env"
```

Check pipeline
```shell
 snakemake -pr --dry-run
# or
 snakemake -pr --dry-run --debug-dag
```

### Run
Run pipeline
```shell
conda activate "$(pwd)/.conda_env"
snakemake -pr --use-conda --cores 6
```
Get bigwigs:
```shell
find bigwigs \( -name "*.bw" -or -name "*.log" \)  | xargs tar -zcvf bigwigs.tar.gz
```

Get peaks:
```shell
find macs2 \( -name "*.narrowPeak" -or -name "*.log" \)  | xargs tar -zcvf peaks.tar.gz
```

Plot DAG and rule graphs
```shell
snakemake --dag | dot -Tsvg > images/dag.svg
snakemake --rulegraph | dot -Tsvg > images/rulegraph.svg
```

## Run analysis:

All subsequent analysis is in the file `Consensus_peaks_Methylation.ipynb`. Run it using `jupyter` package.