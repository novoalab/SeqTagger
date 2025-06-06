# CHANGELOG

## v1.0d
- new demux models:
  - mRNA: b96_RNA004

## v1.0c
- multithreading via Process with Queue
- support for encrypted models
- support for new GPUs (pytorch v2.1.2 / cuda v12.1)
- removed b100_RNA002

## v1.0b
- tRNA support via alignment-based segmentation
- new demux models:
  - mRNA: b96_RNA002 and b100_RNA002
    - deprecated b12_RNA002
  - tRNA: b04_RNA002_tRNA
- added `bam_split_by_barcode.py` that splits BAM files by barcode
- removed multithreading support: it's not memory-safe for pod5 files that may have millions of entries

## v1.0a3
- pod5 support
- fast5_split_by_barcode.py added

## v1.0a2
- fixed several critical issues
- demux models: b12_RNA002

## v1.0a
- mRNA support via signal-based segmentation using T-test with two rolling windows
- Fast5 support
- demux models: b04_RNA002 and b04_RNA004




