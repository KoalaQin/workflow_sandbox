# WGS-SV Pipeline

A Snakemake-based workflow for aligning whole-genome sequencing (WGS) reads and calling structural variants (SVs) using BWA and Manta.

## Pipeline overview:

1. FASTQ validation step
    * Using ValidateFastq to ensure FASTQ files (single or paired) are correctly formatted by checking read integrity, duplicates, header consistency, read counts, and quality encoding. It also reports the most likely quality score format (e.g., Sanger, Solexa).

2. Reference genome indexing with BWA and samtools

3. Read alignment with bwa mem

    * BAM preprocessing: fixmate → sort → duplicate marking

4. Structural variant calling with Manta
    * So far, this pipeline only implemented Manta, but there are other tools available for SV calling (e.g., Delly, Lumpy) and they have been used as an ensemble method to improve sensitivity and specificity.

5. Export of SV calls to CSV format

    * Example data (chr21.fa, test_R1.fastq.gz, test_R2.fastq.gz) included for testing the GitHub Actions workflow.

## Repository Structure
```
├── CONTRIBUTING.md          # Contribution guidelines
├── README.md                # This file
├── requirements-dev.in      # Development requirements (unpinned)
├── requirements-dev.txt     # Development requirements (pinned)
└── wgs_sv_pipeline
    ├── Snakefile            # Main Snakemake workflow
    ├── envs                 # Conda environments
        ├── bwa.yaml
        └── manta.yaml
```
## Requirements

* Snakemake ≥ 7.0

* Conda (for environment management)

* local workstation or cluster or gcloud CLI with enough cores and memory depending on the size of the dataset (The test run was done on a 16-core machine for one sample on chr21 within 25 minutes)

Quick Start
1. Clone the repository
```
git clone https://github.com/KoalaQin/workflow_sandbox.git
```

2. Install snakemake in your conda environment
```
conda create -c conda-forge -c bioconda -n snakemake snakemake-minimal
```

3. Run the workflow
Move your files or change the paths in the Snakefile as needed.
```
cd workflow_sandbox/wgs_sv_pipeline
conda run -n snakemake snakemake --cores 16 --use-conda
```

This will produce:

* Alignment files under intermediate_files/alignments/

* Structural variant calls under intermediate_files/sv_results/

* A summary CSV at results/test_manta_sv.csv


## Input Data

To run on your own data:

Place your FASTQ files in the repo root, following the naming convention:
```
sample_R1.fastq.gz
sample_R2.fastq.gz
```

Update the `SAMPLES` variable in Snakefile.

Replace `chr21.fa` with your reference genome (FASTA format).

## Output

CSV: Parsed variant table (results/{sample}_manta_sv.csv). Intermediate files would be removed.

## Contributing

Please read CONTRIBUTING.md for details on coding standards and submitting pull requests.
