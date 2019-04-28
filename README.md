# RNAseq
1. Pipeline Dependencies

To use the rnaSeqCount pipeline, the following dependencies are required:
1.1. Softwares

    Nextflow
    Singularity
    R

1.2. Singularity Containers

    https://www.singularity-hub.org/collections/770

1.3. Reference Genome and Indexes

    Reference Genome (.fa) and Genome Annotation (.gtf) files
    Reference Genome Indexes (bowtie2 & STAR - see 3. below on how to generate)

2. Optaining the nf-rnaSeqCount pipeline

The nf-rnaSeqCount pipeline can be obtain using any of the following methods:
2.1. Using the git command (recommended):

    git clone https://github.com/phelelani/nf-rnaSeqCount.git

2.2. Using the nextflow command:

    nextflow pull phelelani/nf-rnaSeqCount
    nextflow pull https://github.com/phelelani/nf-rnaSeqCount.git
    nextflow clone phelelani/nf-rnaSeqCount <target-dir>

3. Generating genome indexes.

To generate the STAR and bowtie2 indexes for the reference genome, the Singularity containers first need to be downloaded from Singularity Hub. The prepareData.nf script can be used to download and prepare data (generate indexes) to be be used with the nf-rnaSeqCount pipeline. The prepareData.nf can be run in three different modes:

    getContainers: for downloading the required Singularity containers.
    generateStarIndex: for generating STAR indexes.
    generateBowtieIndex: for generating bowtie2 indexes.

To generate the genome indexes, run the following commands in the pipeline directory:
3.1 Download Singularity containers:

nextflow run prepareData.nf --mode getContainers -profile pbsPrepare

3.2. Generate STAR index

nextflow run prepareData.nf --mode generateStarIndex -profile pbsPrepare

3.3. Generate bowtie2 index

nextflow run prepareData.nf --mode generateBowtieIndex -profile pbsPrepare

NB: The -profile option can either be one of depending on the scheduler.
4. Pipeline Execution

The nf-rnaSeqCount pipeline can be run in one of two ways:
4.1. By editing the parameters.config file and specifying the parameters (recommended)

Edit main.config:

/*
 *  USE THIS FILE TO SPECIFY YOUR PARAMETERS. ALLOWED PARAMETERS ARE AS FOLLOWS:
 *  ============================================================================
 *  data      : Path to where the input data is located (where fastq files are located).
 *  filetype  : Extension of the input FASTQ files (fastq | fq | fastq.gz | fq.gz | fastq.bz2 | fq.bz2).
 *  out       : Path to where the output should be directed (will be created if it does not exist).
 *  genome    : The whole genome sequence (fasta | fa | fna).
 *  index     : Path to where the STAR index files are locaded.
 *  genes     : The genome annotation file (gtf).
 *  bind      : Paths to be passed onto the singularity image (Semi-colon separated).
 *  help      : Print out help menu. Passed as "--help" to the "main.nf" script for detailed information
 */
params {
    data      = "/path/to/data"
    out       = "/path/to/output"
    filetype  = "fastq.gz"
    genome    = "/path/to/genome.fa"  
    index     = "/path/to/STARIndex"
    genes     = "/path/to/genes.gtf" 
    bind      = "/path/to/bind_1;/path/to/bind_2"
    help      = null
}

Then run the pipeline:

nextflow run main.nf

4.2. Directly from the command line by supplying the required parameters

nextflow run main.nf \
    --data "/path/to/data" \
    --filetype "filetype"
    --out "/path/to/output" \
    --genome "/path/to/genome.fa" \
    --index "/path/to/STARIndex" \
    --genes "/path/to/genes.gtf" \
    --bind "/path/to/bind_1;/another/path/to/bind_2"
