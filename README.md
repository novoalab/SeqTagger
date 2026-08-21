![alt text](./img/logo.png "SeqTagger")
![Docker Pulls](https://img.shields.io/docker/pulls/lpryszcz/seqtagger?logo=docker)
[![DOI:10.1101/gr.279290.124](http://img.shields.io/badge/DOI-110.1101/gr.279290.124-blue.svg)](https://doi.org/10.1101/gr.279290.124)
[![X Account](https://img.shields.io/badge/@novoalab-blue?logo=x&logoColor=white&labelColor=555)](https://x.com/novoalab?lang=en)
[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## Table of Contents
- [About SeqTagger](#About-SeqTagger)
- [Models available](#Models-available)
- [Dependencies and versions](#Dependencies-and-versions) 
- [License information](#License-information) 
- [Citation](#Citation) 

## About SeqTagger 
SeqTagger is a super-fast and accurate demultiplexing algorithm for direct RNA nanopore sequencing datasets.
It supports the current sequencing chemistry (SQK-RNA004), and the deprecated chemistry (SQK-RNA002). 
SeqTagger expects reads in standard nanopore sequencing data format (pod5 or fast5). 

Running SeqTagger is as easy as:

```bash
docker run --gpus all -u $UID:$GID -v `pwd`:/data lpryszcz/seqtagger run -k models/b04_RNA004 -r -i /data/input_directory_with_reads -o /data/outdir
```

You can see available demultiplexing models by executing
```bash
docker run --gpus all -u $UID:$GID -v `pwd`:/data lpryszcz/seqtagger ls -lah models
```

For more details see [docs/DESCRIPTION.md](docs/DESCRIPTION.md), for example: 
- [How does SeqTagger work?](docs/DESCRIPTION.md#How-does-SeqTagger-work)
- [How many barcodes are supported?](docs/DESCRIPTION.md#How-many-barcodes-are-supported)
- [Running SeqTagger](docs/DESCRIPTION.md#Running-SeqTagger)
- [Benchmarking of b96_RNA004](docs/DESCRIPTION.md#Benchmarking-of-b96_RNA004)
- [Available models](https://github.com/novoalab/SeqTagger/tree/main/models)

## Models available

Please note that trained models are back-compatible. If you use SeqTagger v2+, all previous models (v1) and latest models (v2) will work.

| Chemistry	|RNA biotype	|Number of barcodes	|Model |
| ----------- | ----------- | ----------- | ----------- |
|RNA004|	mRNA (or in vitro polyadenylated RNA)|	4|	b04_RNA004|
|||13|	b13_RNA004_mRNA|
|||96|	b96_RNA004|
||tRNA|	7|	b07_RNA004_tRNA|


If you're still interested in demuxing RNA002 runs:

| Chemistry	|RNA biotype	|Number of barcodes	|Model |
| ----------- | ----------- | ----------- | ----------- |
|RNA002	|mRNA (or in vitro polyadenylated RNA)|	4	|b04_RNA002|
|||96	|b96_RNA002|
||tRNA|	4|	b04_RNA002_tRNA|

### Barcode sets

Please note, barcode sets change depending on the model:

** a) **b04 models** (b04_RNA002, b04_RNA004 and b04_RNA002_tRNA) models are demuxing the same 20nt- barcodes as DeePlexiCon, listed [HERE](https://github.com/novoalab/SeqTagger/blob/main/models/v1/b04_RNA002/barcodes.fa).

Barcode 1: GGCTTCTTCTTGCTCTTAGG Barcode 2: GTGATTCTCGTCTTTCTGCG Barcode 3: GTACTTTTCTCTTTGCGCGG Barcode 4: GGTCTTCGCTCGGTCTTATT

** b) **b96 models** (b96_RNA002 and b96_RNA004) are using the 37-nt barcodes, listed [HERE](https://github.com/novoalab/SeqTagger/blob/main/models/v1/b96_RNA004/barcodes.fa).

** c) **b07 models** (b07_RNA004_tRNA) are using a mix of 20nt and 37nt barcodes listed [HERE](https://github.com/novoalab/SeqTagger/blob/main/models/v2/b07_RNA004_tRNA/barcodes.fa).

## Dependencies and versions

You'll need CUDA-compatible (Nvidia) GPU and 
[CUDA v10 or newer installed](https://developer.nvidia.com/cuda-downloads) 
in your system supporting **half-precision (float16)**. 
All Nvidia GPUs released from 2019 onward should work without any issues. 

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


## License Information

This project is licensed under the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC-ND 4.0), 
available [here](https://creativecommons.org/licenses/by-nc-nd/4.0/), 
with the exception of the `bonito` module, which retains its original license. 
The full text of the licenses, including modified code, can be found in the `bonito` directory.

License Dependencies:

- **ONT 1.0**: `bonito`
  - Licensed under the Oxford Nanopore Technologies Public License 1.0. Full license text available at [ONT 1.0 License](https://github.com/nanoporetech/bonito/blob/master/LICENCE.txt).
- **MPL 2.0**: `pod5`, `ont_fast5_api`
  - Licensed under the Mozilla Public License 2.0. Full license text available at [MPL 2.0 License](https://www.mozilla.org/en-US/MPL/2.0/).
- **BSD 3-Clause**: `pandas`, `seaborn`, `joblib`, 
  - Licensed under the BSD 3-Clause License. Full license text available at [BSD 3-Clause License](https://opensource.org/licenses/BSD-3-Clause).
- **MIT**: `mappy`, `pysam`, `numpy`
  - Licensed under the MIT License. Full license text available at [MIT License](https://opensource.org/licenses/MIT).
- **OTHER**: `pytorch`, `numpy`
  - Full license text for `pytorch` is available at [pytorch License](https://github.com/pytorch/pytorch/blob/main/LICENSE).
  - Full license text for `numpy` is available at [numpy License](https://numpy.org/doc/stable/license.html).

Please ensure compliance with each license's terms and conditions.

LPP, GD and EMN have filed patent applications (EP24382340 and EP24383144) based on this work at the European Patent Office. 


## Citation

If you found this work helpful, please cite:

Pryszcz LP*#, Diensthuber G*, Llovera L,  Medina R, Delgado-Tejedor A, Cozzuto L, Ponomarenko J and Novoa EM#.
[**Rapid and accurate demultiplexing of direct RNA nanopore sequencing datasets with SeqTagger**](https://doi.org/10.1101/gr.279290.124). 
Genome Research 2025 



