# Data associated with a tutorial on reproducibility


## The tutorial
The tutorial that this data belongs to can be found [here](https://reproducible.sschmeier.com/) ([https://reproducible.sschmeier.com/](https://reproducible.sschmeier.com/)).

## How to use this repository
This is a directory structure to develop a Snakemake workflow. It contains all the data (see below)that is needed. A Snakemake workflow is based on a "Snakefile" that contains rules on how to process certain types of data. **The aim of the tutorial is to develop a "Snakefile" to analyse the samples in the fastq directory.**

There are some examples of how a Snakefile can be developed in the "examples" directory.

## Data

### Samples
The data is from a transcriptomics experiment in yeast and has been downsampled heavily to facilitate quick analyses.
The original data can be found at the Short Read Archive ([https://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&from_uid=212389](https://reproduce.sschmeier.com)).

-  [https://www.ncbi.nlm.nih.gov/sra/SRX362638](https://www.ncbi.nlm.nih.gov/sra/SRX362638)
-  [https://www.ncbi.nlm.nih.gov/sra/SRX362639](https://www.ncbi.nlm.nih.gov/sra/SRX362639)
-  [https://www.ncbi.nlm.nih.gov/sra/SRX362640](https://www.ncbi.nlm.nih.gov/sra/SRX362640)
-  [https://www.ncbi.nlm.nih.gov/sra/SRX362641](https://www.ncbi.nlm.nih.gov/sra/SRX362641)
-  samples: [SRR941826, SRR941827, SRR941830, SRR941831]

### Genome

- [ftp://ftp.ensembl.org/pub/release-92/fasta/saccharomyces_cerevisiae/dna/Saccharomyces_cerevisiae.R64-1-1.dna_sm.toplevel.fa.gz](ftp://ftp.ensembl.org/pub/release-92/fasta/saccharomyces_cerevisiae/dna/Saccharomyces_cerevisiae.R64-1-1.dna_sm.toplevel.fa.gz)


### Genome Annotation

- [ftp://ftp.ensembl.org/pub/release-92/gtf/saccharomyces_cerevisiae/Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz"](ftp://ftp.ensembl.org/pub/release-92/gtf/saccharomyces_cerevisiae/Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz")


## Running Snakemake

```bash
# create snakemake env
conda env create -n snakemake --file envs/snakemake.yaml

# activate env
conda activate snakemake

# dry run
snakemake -np --use-conda

# execute
snakemake -Tp --use-conda
```
## Examples

### Directory content

```bash
examples/
├── dag_v5.png    ## workflow diagram for v5
├── dag_v6.png    ## workflow diagram for v6
├── dag_v7.png    ## workflow diagram for v7
├── Snakefile_v2  ## workflow with one rule (trimming) and explicit naming of targets
├── Snakefile_v3  ## workflow with one rule (trimming) and target file detection based on samples 
├── Snakefile_v4  ## workflow with one rule (trimming) + logging and benchmarking of the rule
├── Snakefile_v5  ## workflow with one rule (trimming) + specific conda environment for rule
├── Snakefile_v6  ## workflow with three rules (trimmming, genome indexing and read mapping)
├── Snakefile_v7  ## complete workflow, conda-based rule execution
├── Snakefile_v8  ## complete workflow, singularity container based rule execution
└── Snakefile_v9  ## workflow with one rule (trimming), readiong samples from Google Cloud Storage
```

### Running examples

```bash
# use singularity containers
snakemake -Tp --use-singularity --snakefile examples/Snakefie_v8

# use singularity locally but get smaples from GS bucket and put results to bucket as well
snakemake -Tp --use-singularity --default-remote-provider GS --default-remote-prefix schmeier-reproduce-bucket --snakefile examples/Snakefile_v9
```

### NeSI

```bash
# using Singularity, only if set up on cluster
snakemake --use-singularity --singularity-args "--bind /scale_wlg_nobackup/filesets/nobackup/PROJECTNUMBER" -j 999 --cluster-config data/nesi/cluster-nesi-mahuika.yaml --cluster "sbatch -A {cluster.account} -p {cluster.partition} -n {cluster.ntasks} -t {cluster.time} --hint={cluster.hint} --output={cluster.output} --error={cluster.error} -c {cluster.cpus-per-task} --mem={cluster.mem}" -p --rerun-incomplete
```