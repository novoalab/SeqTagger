# Description

## Table of Contents
- [About-SeqTagger](#About-SeqTagger)
  - [What is SeqTagger?](#What-is-SeqTagger)
  - [How does SeqTagger work?](#How-does-SeqTagger-work)
  - [How many barcodes are supported?](#How-many-barcodes-are-supported)
  - [Does it work on all RNA types?](#Does-it-work-on-all-RNA-types)
- [Running SeqTagger](#Running-SeqTagger)
  - [Split reads by barcode](#Split-reads-by-barcode)
- [Benchmarking of b96_RNA004](#Benchmarking-of-b96_RNA004)

## About SeqTagger

### What is SeqTagger? 
It's a super-fast and accurate demultiplexing algorithm for direct RNA nanopore sequencing datasets.
It supports the current sequencing chemistry (SQK-RNA004), and the depcracated chemistry (SQK-RNA002). 
Moreover, both standard file formats for nanopore sequencing data (pod5 and fast5) are supported.


### How does SeqTagger work? 

The workflow follows the standard direct RNA sequencing library preparation protocol 
in which default RT adapters are exchanged for barcode-containg RT adapters. 
SeqTagger then basecalls the DNA barcode from the direct RNA sequencing data using custom basecalling models. 
Finally, basecalled barcodes are aligned against the reference sequences 
for all barcodes and low confidence predicitions removed in a filtering step. 

#### v1

SeqTagger v1 first trims barcode signal by detecting poly(A)-tail signal (segmantation step). 
Then barcode signal is normalised (medmad), 
last part of barcode signal is basecalled, 
resulting sequence is aligned to expected barcode sequences
and best match is reported. 

This works well for reads with long and "clean" poly(A)-tail signals, 
for example for standard mRNA transcripts or polyadenylated total RNA, 
but may fail for reads with no, short or non-standard tails. 

![alt text](/img/workflow.png "SeqTagger_Workflow")

#### v2

SeqTagger v2 first standarises read signal 
and basecalls every read several times using rolling-window approach. 
Basecalled sequence of the window with with the highest mean basecalling quality (baseQ) 
is aligned to expected barcode sequences and best match is reported. 

SeqTagger v2 handles better reads with non-standard tails (no, short or non-standard tail), 
but it is 3-6x slower than v1, because every read is basecalled several times. 

### How many barcodes are supported?
Currently, SeqTagger supports the following models and barcodes:

| SeqTagger version | Chemistry | Number of barcodes | SeqTagger Model | Barcode Sequences | 
| :------: | :------: | :------: | :------: | :------: |
| 1 | SQK-RNA002 | 4 | b04_RNA002 | [b04_RNA002_barcodes](/models/b04_RNA002/barcodes.tsv)|
| 1 | SQK-RNA002 | 96 | b96_RNA002 | [b96_RNA002_barcodes](/models/b96_RNA002/barcodes.tsv)|
| 1 | SQK-RNA004 | 4 | b04_RNA004 |  [b04_RNA004_barcodes](/models/b04_RNA004/barcodes.tsv)|
| 1 | SQK-RNA004 | 96 | b96_RNA004 | [b96_RNA004_barcodes](/models/b96_RNA004/barcodes.tsv)|
| 2 | SQK-RNA004 | 7 | b07_RNA004_tRNA | [b07_RNA004_tRNA_barcodes](/models/b07_RNA004_tRNA/barcodes.tsv)|
| 2 | SQK-RNA004 | 13 | b13_RNA004_mRNA | [b13_RNA004_mRNA_barcodes](/models/b13_RNA004_mRNA/barcodes.tsv)|

**Please note:** The barcode sequences used for the b04 and b96 model are identical between the two chemistries SQK-RNA004 and SQK-RNA002.


### Does it work on all RNA types?
Yes, as long as the RNA molecule has a poly(A) tail (e.g. mRNAs, lncRNAs, etc.) or you have in vitro polyadenylated the sample prior to sequencing.

**Please note**: Nano-tRNAseq libraries do not have standard poly(A) RNA tails, and thus should not be used with the models listed above. You can find SeqTagger Dockerfiles with pre-trained **tRNA** demultiplexing models [here](https://www.immaginabiotech.com/nano-trnaseq-kit/ntrsq-12) (also available for **RNA004** chemistries).


## Running SeqTagger

Download test data for both SQK-RNA004 and SQK-RNA002:

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
seqtagger run -k models/b04_RNA004 -r -i /data/test_data/RNA004 -o /data/demux
```
Note, you can provide multiple input directories with fast5/pod5 files after `-i`. 

Results will be saved in tab-delimited files (gzip-compressed): 
`demux/RNA004.demux.tsv.gz`

In addition, boxplots of per-barcode quality will be saved in corresponding directory
ending with `.boxplot.pdf`. 

**Please note**:
You can now also run SeqTagger through the [MasterOfPores3](https://github.com/biocorecrg/master_of_pores) nextflow workflow. 

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

## Benchmarking of b96_RNA004

The confusion matrix below illustrates performance of the b96_RNA004 model on the holdout dataset, reaching >0.99 precision (recall = 1):

![alt text](./img/cm_b96_RNA004.png "cm_b96_RNA004")

We further tested b96_RNA004 by performing a dropout test in which three randomly chosen barcodes were skipped during library generation (SCBC-21, SCBC-59, and SCBC-88).
This yielded an average cross-contamination rate per barcode of less than 0.00002% of the total library (**A**). We further compared the runtime of b04_RNA004 to that of the
larger b96_RNA004 model and observed no signficant differences (**B**).

**Please note**: This performance was obtained on a network filesystem, substantially faster times can be achieved on a local SSD or HDD.

![alt text](./img/indep_test_and_realtime_v2.png "indep_test_and_realtime_v2")

