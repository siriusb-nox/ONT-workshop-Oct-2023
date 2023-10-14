## A. bascalling from fast5 files using `guppy` (legacy)

`guppy` is one of the _official programs_ designed by Oxford Nanopore to translate the electrical signals contained in the raw FAST5 files into nucleotides (FASTQ files). This program is only accessible if you have a client account with ONT. This program has been succeded by `dorado` (see section below).
Base calling is done directly when sequencing a library in the minION (i.e., _live basecalling_), but this process is computationally intensive and there are some advantages of base calling post sequencing (e.g., running highly accurate base calling models using a powerful computer).

The `guppy` installer files comes in two flavours: **CPU** (uses processor resources) and **GPU** (uses graphics card resources - available for a limited type of cards, e.g., NVIDIA). The GPU version is much more efficient than the CPU. 

To run guppy, you can run the following command:

```bash
ls /directory/staff/*.fast5 | guppy_basecaller -s . -c dna_r9.4.1_e8.1_hac.cfg --compress_fastq --trim_adapters -x auto --min_qscore 10
```

Where:

```bash
-s # specifies the directory to store the analysis outputs
-c # indicates the model to call the bases (dna_r9.4.1_e8.1_hac.cfg is one of the most accurate models)
-x # enable the option to automatically detect which GPU device is available on the computer
--min_qscore 10 # will exclude any DNA sequences with a quality score less than 10 
```

An idea of how much GPU/CPU resources different base calling models consume is available [here](https://esr-nz.github.io/gpu_basecalling_testing/gpu_benchmarking.html#cfg_files).

Whenever `guppy` has ran successfully basecalling, the program prints a message like the following one:

```bash
usuario@maquina:/home/selamatpagi/Downloads/software/guppy/ont-guppy/bin/guppy_basecaller -s . -c dna_r9.4.1_e8.1_hac.cfg --compress_fastq --trim_adapters -x auto --min_qscore 10
ONT Guppy basecalling software version 3.1.5+781ed57
config file:        /home/selamatpagi/Downloads/software/guppy/ont-guppy/data/dna_r9.4.1_450bps_hac.cfg
model file:         /home/selamatpagi/Downloads/software/guppy/ont-guppy/data/template_r9.4.1_450bps_hac.jsn
input path:         raw_data/
save path:          MyGenome_basecalled_20190621_hac/
chunk size:         1000
chunks per runner:  1000
records per file:   4000
fastq compression:  ON
num basecallers:    4
gpu device:         cuda:0
kernel path:
runners per device: 2

Found 1215050 fast5 files to process.
Init time: 12755 ms

0%   10   20   30   40   50   60   70   80   90   100%
|----|----|----|----|----|----|----|----|----|----|
***************************************************
Caller time: 12227895 ms, Samples called: 117512178322, samples/s: 9.61017e+06
Finishing up any open output files.
Basecalling completed successfully.
```

In addition to the basecalled fastq files, `guppy` also produces a number of reports, including a compilation of sequencing stats in html format. An example can be seen [here:](http://htmlpreview.github.io/?https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/guppy/report_FAV15499_20231010_1636_deec7cb2.html)

*here is a screenshot of the report (taken from the sequencing experiment Art_altilis_DRY_WGS17):*
<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/guppy_report_example_Art_altilis.png" alt="A section of a guppy report on a seq experiment"/>
</p>

## Basecalling using `dorado` (Open Access software)
This software is replacing `guppy` (available [here](https://github.com/nanoporetech/dorado)). `dorado` offers a wide range of analysis, including basecalling (single and duplex), modified basecalling (detection of modified bases) and simplex barcoding classification (useful when sequencing multiple samples per seq experiment). One important point is that, unlike guppy, this software is OA.

One requirement that dorado has is the usage of **POD5** input files (now produced by the newest sequencing machines and chemistries, they will replace eventually *.fast5 files).

```bash
# check list of available models
dorado download --list

# download chosen basecalling model
dorado download --model dna_r10.4.1_e8.2_400bps_hac@v4.2.0

# basecall from pod5
dorado basecaller --emit-fastq dna_r10.4.1_e8.2_400bps_hac@v4.2.0 FAT98192_pass_deec7cb2_ec30cd82_101.pod5 > FAT98192_pass_deec7cb2_ec30cd82_101.fastq
```

Where:

```bash
--emit-fastq # produces as output *.fastq files
dna_r10.4.1_e8.2_400bps_hac@v4.2.0 # indicates the model to call the bases (this is the most accurate model)
FAT98192_pass_deec7cb2_ec30cd82_101.pod5 # input file
```

**NB: The default command does not use any quality filtering threshold (i.e., `--min-qscore 0`)**

The program will repeat the following message:
```bash
[2023-10-13 16:27:08.374] [info] > Creating basecall pipeline
[2023-10-13 16:27:08.457] [info]  - set batch size to 1824
[2023-10-13 16:28:16.576] [info] > Simplex reads basecalled: 4100
[2023-10-13 16:28:16.576] [info] > Simplex reads filtered: 1
[2023-10-13 16:28:16.576] [info] > Basecalled @ Samples/s: 1.783569e+06
[2023-10-13 16:28:16.681] [info] > Finished
```

### ACTIVITY
1. Basecall again reads from the file FAT98192_pass_deec7cb2_ec30cd82_101.pod5, but using a quality threshold value of 20 (parameter `--min-qscore 20`).
2. Count how many low quality reads were excluded by comparing the number of reads produced by the default settings command (i.e., `--min-qscore 0`) and by applying a quality threshold value  

