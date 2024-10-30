![alt text](./img/logo.png "SeqTagger")

## Table of Contents
- [About-SeqTagger](#About-SeqTagger)
- [Running SeqTagger](#Running-SeqTagger)
  - [Split reads by barcode](#Split-reads-by-barcode)
- [Dependencies and versions](#Dependencies-and-versions)
- [Citation](#Citation)

## About SeqTagger

### What is SeqTagger? 
It's a super-fast and accurate demultiplexing algorithm for direct RNA nanopore sequencing datasets.
Supporting both RNA002 and RNA004 kits,and both fast5 and pod5 files. 

### How does SeqTagger work? 
The workflow follows the standard direct RNA sequencing library preparation protocol in which default RT adapters are exchanged for barcode-containg RT adapters. SeqTagger then basecalls the DNA barcode from the direct RNA sequencing data using custom basecalling models. Finally, basecalled barcodes are aligned against the reference sequences for all barcodes and low confidence predicition removed in a filtering step. 

![alt text](./img/workflow.png "SeqTagger_Workflow")

### How many barcodes are supported?
Currently, SeqTagger supports the following models and barcodes:


| Chemistry | Number of barcodes | SeqTagger Model | Barcode Sequences | 
| -----------| ----------- | ----------- |----------- |
| RNA002 | 4 | b04_RNA002 | [b04_RNA002_barcodes](/models/b04_RNA002/barcodes.tsv)|
| RNA002 | 96 | b96_RNA002 | [b96_RNA002_barcodes](/models/b96_RNA002/barcodes.tsv)|
| RNA004 | 4 | b04_RNA004 |  [b04_RNA004_barcodes](/models/b04_RNA002/barcodes.tsv)|


Please note: These models do not work well on Nano-tRNAseq libraries. **tRNA** models for **RNA002** and **RNA004** chemistries are available via [Immagina Biotechnology](https://www.immaginabiotech.com/services/nano-trnaseq) website.


### Does it work on all RNA types?
Yes, as long as the RNA molecule has a poly(A) tail (e.g. mRNAs, lncRNAs, etc.) or you have in vitro polyadenylated the sample prior to sequencing.

**Please note**: NanotRNAseq libraries cannot be directly demuxed using this version of SeqTagger, because the adapter sequences have RNA-DNA hybrids. To demultiplex Nano-tRNAseq libraries please use the SeqTagger Dockerfile available [here](https://immaginabiotech.com/nano-trnaseq) with pre-trained models to demultiplex tRNAs.


## Running SeqTagger

Download test data for both RNA002 and RNA004:

```bash
mkdir -p seqtagger; cd seqtagger
wget https://public-docs.crg.es/enovoa/public/seqtagger/test_data/ \
  -q --show-progress -r -c -nc -np -nH --cut-dirs=3 --reject="index.html*"
```

It's handy to define an alias prior to using `seqtagger`:

```bash
alias seqtagger="docker run --gpus all -u $UID:$GID -v `pwd`:/data lpryszcz/seqtagger"
```
This will bind your current directory to `/data` in the docker container.

Then, running it is as easy as:

```bash
seqtagger mRNA -k models/b04_RNA004 -r -i /data/test_data/RNA004 -o /data/demux
```
Note, you can provide multiple input directories with fast5/pod5 files after `-i`. 

Results will be saved in tab-delimited files (gzip-compressed): 
- `demux/RNA004.demux.tsv.gz`

In addition, boxplots of per-barcode quality will be saved in corresponding directory
ending with `.boxplot.pdf`. 

**Please note**:
You can now also run SeqTagger through the [MasterOfPores3](https://github.com/biocorecrg/MoP3/tree/master) nextflow workflow. 

### Split reads by barcode

#### Split Fast5 files

If you wish to split Fast5 file(s) by barcode, execute:

```bash
seqtagger fast5_split_by_barcode.py -b 50 -i /data/demux/RNA004.demux.tsv.gz \
  -f /data/test_data/RNA004 -o /data/demux/RNA004 
```

Where `-b` specifies the baseQ cut-off. This will generate one output folder for each barcode named as
`demux/RNA004/bc_?/reads_*.fast5` where `?` represents the barcode number.

#### Split FastQ files

We don't provide FastQ example in the test_data. 
If you wish to split FastQ file(s) by barcode:

```bash
# first concatenate all FastQ file into one
cat guppy/run1/*.fastq.gz > guppy/run1.fastq.gz
# and split reads using baseQ cut-off of 50
seqtagger fastq_split_by_barcode.py -b 50 -o /data/demux/run1 -i /data/demux/run1.demux.tsv.gz -f /data/guppy/run1.fastq.gz
```

This will save one FastQ file for each barcode named as
`demux/run1.demux.bc_?.fq.gz` where `?` represents the barcode number.

#### Split BAM files

We don't provide BAM example in the test_data. 
If you wish to split BAM file(s) by barcode:

```bash
seqtagger bam_split_by_barcode.py -i /data/demux/run1.demux.tsv.gz -f /data/run1.mapped.bam -o /data/run1.mapped
```

This will save one BAM file for each barcode named as
`run1.mapped.bc_?.bam` where `?` represents the barcode number.


## Dependencies and versions

You'll need CUDA-compatible (Nvidia) GPU and 
[CUDA v10 or newer installed](https://developer.nvidia.com/cuda-downloads) 
in your system. 

Additionally, you'll need to install 
[docker](https://www.docker.com/)
and 
[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html). 

Versions tested: 
| Software    | Version     | 
| ----------- | ----------- |
| CUDA        | 10, 11, 12  | 
| Docker      | 25+         | 
| Nvidia Container Toolkit | 1.14 | 

## Citation

Pryszcz LP, Diensthuber G, Medina R, Llovera L, Liu H, Delgado-Tejedor A, Cozzuto L and Novoa EM.
**SeqTagger: a rapid and accurate tool to demultiplex direct RNA nanopore sequencing datasets**.
In preparation. 

