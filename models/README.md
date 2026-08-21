## Models available

Please note that trained models are back-compatible. 
If you use SeqTagger v2+, all previous models (v1) and latest models (v2) will work.

| Chemistry |  RNA biotype | Number of barcodes | Model |
|-----------|-----------|-----------|-----------|
|RNA004 |  mRNA (or in vitro polyadenylated RNA)   | 4   | b04_RNA004 |
| |     | 13   | b13_RNA004_mRNA |
| |   | 96   | b96_RNA004 |
| |  tRNA   | 7   | b07_RNA004_tRNA |

If you're still interested in demuxing RNA002 runs: 

| Chemistry |  RNA biotype | Number of barcodes | Model |
|-----------|-----------|-----------|-----------|
|RNA002 |  mRNA (or in vitro polyadenylated RNA)   | 4   | b04_RNA002 |
| |     | 96   | b96_RNA002 |
| |   tRNA  | 4   | b04_RNA002_tRNA |

## Barcode sets

Please note, barcode sets change depending on the model: 

** a) **b04 models** (b04_RNA002, b04_RNA004 and b04_RNA002_tRNA) models are demuxing the same 20nt- barcodes as DeePlexiCon, listee [HERE](https://github.com/novoalab/SeqTagger/new/main/models/v1/b04_RNA004/barcodes.fa). 

Barcode 1: GGCTTCTTCTTGCTCTTAGG
Barcode 2: GTGATTCTCGTCTTTCTGCG
Barcode 3: GTACTTTTCTCTTTGCGCGG
Barcode 4: GGTCTTCGCTCGGTCTTATT

** b) **b96 models** (b96_RNA002 and b96_RNA004) are using the 37-nt barcodes, listed [HERE](https://github.com/novoalab/SeqTagger/new/main/models/v1/b96_RNA004/barcodes.fa).

** c) **b07 models** (b07_RNA004_tRNA) are using a mix of 20nt and 37nt barcodes listed [HERE](https://github.com/novoalab/SeqTagger/new/main/models/v2/b07_RNA004_tRNA/barcodes.fa).



