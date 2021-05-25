# replication-fork-project
IB 2021 Spring Student Project

# Run pipeline:

## Prerequisites

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

## Run
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