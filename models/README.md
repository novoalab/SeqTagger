# Models

## Table of Contents
- [Available models](#Available-models)
- [How to build barcoded direct RNA sequencing libraries that can be demuxed with SeqTagger](#How-to-build-barcoded-direct-RNA-sequencing-libraries-that-can-be-demuxed-with-SeqTagger)

## Available models

For **RNA004** kits (RNA flowcells):
* b04_RNA004: demultiplexing model to demux any type of pA-tailed RNA (e.g. mRNAs or pA-tailed IVTs)
* tRNA models are available via [Immagina](https://www.immaginabiotech.com/services/nano-trnaseq)

For **RNA002** kits (R9.4 flowcells):
* b04_RNA002: demultiplexing model to demux any type of pA-tailed RNA (e.g. mRNAs or pA-tailed IVTs)
<<<<<<< HEAD
* b96_RNA002: demultiplexing model to demux any type of pA-tailed RNA (e.g. mRNAs or pA-tailed IVTs)
* b100_RNA002: demultiplexing model to demux any type of pA-tailed RNA (e.g. mRNAs or pA-tailed IVTs)
* b04_RNA002_tRNA: demultiplexing model that for tRNAs prepared using [Nano-tRNAseq](https://www.nature.com/articles/s41587-023-01743-6) approach.
  * more tRNA models are available via [Immagina](https://www.immaginabiotech.com/services/nano-trnaseq)

Barcode numbers are given in `barcodes.fa` file.   
You can find corresponding A/B oligo sequences to each barcode in `barcodes.tsv` file.
For example, barcode 1 in corresonds to SCBC-01 in `b96_RNA002` model:

```bash
$ grep SCBC-01 -C1 b96_RNA002/barcodes.{fa,tsv}
b96_RNA002/barcodes.fa:>Barcode_1 SCBC-01
b96_RNA002/barcodes.fa-GTTTATAGAAAACGTTTTGAAGAAGAAGATGATCTCT
--
b96_RNA002/barcodes.tsv-BARCODE_ID      demultiplexing model    OligoA  OligoB
b96_RNA002/barcodes.tsv:SCBC-01 b96_RNA002      /5Phos/GTTTATAGAAAACGTTTTGAAGAAGAAGATGATCTCTTAGTAGGTTC  GAGGCGAGCGGTCAATTTTAGAGATCATCTTCTTCTTCAAAACGTTTTCTATAAACTTTTTTTTTT
b96_RNA002/barcodes.tsv-SCBC-02 b96_RNA002      /5Phos/GTGTCCTACTATGTCTTCTCTCTTCTACTACTTACCTTAGTAGGTTC  GAGGCGAGCGGTCAATTTTAGGTAAGTAGTAGAAGAGAGAAGACATAGTAGGACACTTTTTTTTTT
```
=======
* b04_RNA002_tRNA: demultiplexing model that for tRNAs prepared using [Nano-tRNAseq](https://www.nature.com/articles/s41587-023-01743-6) approach. 
 
<<<<<<< HEAD
>>>>>>> parent of 78eed35 (update models README)
=======
>>>>>>> parent of 78eed35 (update models README)

### How can I know which models come with my current version? 

You can list available models using:
```bash
seqtagger ls models
```

If you wish to use external models, just bind your model directory:
```bash
alias seqtagger="docker run --gpus all -u $UID:$GID -v DATA_DIR:/data -v MODEL_DIR:/models lpryszcz/seqtagger"
```

Now you can list your custom models
```bash
seqtagger ls /models
```

and use them as follows:
```bash
seqtagger ... -k /models/CUSTOM_MODEL_NAME ...
```

##  How to build barcoded direct RNA sequencing libraries that can be demuxed with SeqTagger

To build the barcoded libraries, the oligo DNA sequences listed below should be used instead of those coming with the direct RNA sequencing kit (RTA). The barcode is embedded in the oligoA sequence, which will be ligated to the RNA molecule during the library preparation.

These oligos are designed to barcode libraries which have been enriched with oligodT beads (i.e. RNA should have a polyA tail to anneal to oligoB). 

| Barcode      | SeqTagger Model | OligoA | OligoB |
| ----------- | ----------- |----------- |----------- |
| BC-01   | b04_RNA002 b04_RNA004 b04_RNA002_tRNA b12_RNA002 | /5Phos/GGCTTCTTCTTGCTCTTAGGTAGTAGGTTC       |GAGGCGAGCGGTCAATTTTCCTAAGAGCAAGAAGAAGCCTTTTTTTTTT       |
| BC-02   | b04_RNA002 b04_RNA004 b04_RNA002_tRNA b12_RNA002 | /5Phos/GTGATTCTCGTCTTTCTGCGTAGTAGGTTC       |GAGGCGAGCGGTCAATTTTCGCAGAAAGACGAGAATCACTTTTTTTTTT       |
| BC-03   | b04_RNA002 b04_RNA004 b04_RNA002_tRNA b12_RNA002 | /5Phos/GTACTTTTCTCTTTGCGCGGTAGTAGGTTC       |GAGGCGAGCGGTCAATTTTCCGCGCAAAGAGAAAAGTACTTTTTTTTTT       |
| BC-04   | b04_RNA002 b04_RNA004 b04_RNA002_tRNA b12_RNA002 | /5Phos/GGTCTTCGCTCGGTCTTATTTAGTAGGTTC       |GAGGCGAGCGGTCAATTTTAATAAGACCGAGCGAAGACCTTTTTTTTTT       |
| BC-05   | b12_RNA002 |    /5Phos/GTTGCGACGATGACTGACGACTGCACGAAAAGCTGGATAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTTCCAGCTTTTCGTGCAGTCGTCAGTCATCGTCGCAACTTTTTTTTTT    |
| BC-06   | b12_RNA002 |    /5Phos/GGTGAGGAGGAGAAGTAAAAGAAAGCTTCGAGAGAGTTAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTTTCTCTCCTCCCTCTCTTTCCTCGTGAGGTGGGACTCTTTTTTTTTT    |
| BC-07   | b12_RNA002 |    /5Phos/GAGTTTACACGGCGCTCTTTCCGGTTTGATCTTGCACTAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTTATTATTTCAACCAGCTTCCCTCTCGCATCCAGCTGCTTTTTTTTTT    |
| BC-08   | b12_RNA002 |    /5Phos/GCTGTTTGCACACACACCCGCACACCCTGTTCCCTCGTAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTGCGTAAAAGTCCCGAGTTTCCTCCTCCCTCTTCGCACTTTTTTTTTT    |
| BC-09   | b12_RNA002 |    /5Phos/GATGCGTTGCGTTGTTTTGCGTTCCACACCACACGTTTAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTAGTGTAAAAGACAAAACGAGAAAAGCGTGCGCGGAACTTTTTTTTTT    |
| BC-10   | b12_RNA002 |    /5Phos/GATGTTTACGCACGCGTTTTCCCACCCACGATGTTGTTAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTGTGGGCCTCTCCCTCGGAGAGTGTACCGCTCTTTCCCTTTTTTTTTT    |
| BC-11   | b12_RNA002 |    /5Phos/GATCCAAAGAGAACTGGGATTTCTAAAAGAGAGAGAATAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTCCCTTTGACTCTCTCTCTCCCTCTCCTCTTTGTGAACTTTTTTTTTT    |
| BC-12   | b12_RNA002 |    /5Phos/GAGTCCCACCTCACGAGGAAAGAGAGGGAGGAGAGAATAGTAGGTTC    |   GAGGCGAGCGGTCAATTTTAGAGATCATCTTCTTCTTCAAAACGTTTTCTATAAACTTTTTTTTTT    |
