# replication-fork-project

IB 2021 Spring Student Project

# Goal of the project

The aim of the project is to investigate methylation patterns changes in replication origins regions in people of
different age groups.

# Objectives

The main objectives of the project are:

* create a Snakemake pipeline to get consistent data
* develop a method for determining conservative peaks
* select methylation datasets for different age groups
* correlate methylation data with replication origins regions
* assess methylation signal changes and methylation variability

# Methods

To download replication origins data and further peak calling, we have developed a pipeline using Snakemake.
The data source of ChIP-Seq and SnS-seq experiments are presented in `config/replication_origins_data.csv`.

Further, with the obtained peaks, an analysis was carried out to find a consensus on the replication origins from all
peak tracks.

After that, we used the obtained conservative peaks to check the correlation of differential methylation between young
and old groups in the obtained regions. As a source of methylation data, we used the RRBS data, where methylation level
was measured in CpG-islands for CD14+ CD16- monocytes of two cohorts of healthy young and older donors. Using the data
obtained, we carried out an analysis of differential methylation using the Mann-Whitney test with FDR 0,05. The gene
sets of significantly differentially methylated regions (we link targeted replication origins of the genome with
neighboring genes using GREAT) then have been matched against biological pathways and Gene Ontology databases.

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

# Links and references

* [RRBS data](https://artyomovlab.wustl.edu/aging/download_data.html#download-methylation)
* [GREAT](http://bejerano.stanford.edu/great/public/html/index.php)
* [GEne SeT AnaLysis Toolkit](http://www.webgestalt.org)