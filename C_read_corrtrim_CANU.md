## C. ONT DATA CORRECTION AND FILTERING

There are several programs that can operate with ONT reads. In this workshop, we will rely on `canu`, which is a program designed to perform ONT read correction, filtering (removal of bases or fragments from sequences with low quality) and scaffold assembly. The program **requires a lot of resources for its execution** (this depends on the size of the genomes, their complexity and the initial amount of data being used for its execution. **The program recommends starting the run with an amount of data that represents between 20x-60x+ theoretical coverage (e.g., Sun et al., 2021).** The program can operate with lower ammounts of input data but at the expense of higher error rates (higher number of miscalled bases on the read after correction analysis).

*Excerpt from Sun et al.,2021* 
>The following assemblers were used for the benchmarking, including the long-read only assemblers (Canu [12], Flye [13], Wtdbg2 [14], Miniasm [15], NextDenovo >>(https://github.com/Nextomics/NextDenovo), NECAT [6], Raven [16] and Shasta [7]) and hybrid assemblers (MaSuRCA [17] and QuickMerge [18]). **Canu was not tested on the M. coruscus genome owing to the extremely intensive computing time required for this large genome**. Previous analyses have suggested that using corrected ONT reads could improve the genome assembly [19]. To check the effect that this has on the assemblies, the ONT reads that were corrected and/or trimmed by Canu.

canu is a modular program, this means that each mode (_correction_, _trim_, _assembly_) can be run separately. This is recommended as in some instances the program may crash due to lack of resources, especially when assembling complex genomes and using lots of input data (e.g., nuclear genomes of +1 Gb). In this case, it is recommended to use `canu` for corrections and filtering, and other programs for assembling corrected and filtered data (e.g. SMARTdenovo). 

In this section of the workshop, we will correct, trim and assemble a small subset of the _Artocarpus altilis_ dataset sequenced from a sample collected in a dry environment (WGS17). This will enable is to use `canu` for correction, trimming and assembly. We will aim to assemble the plastid genome of _A. altilis!_

The data required to run this section is available in the folder `NGSdat/fastq_subset/` of your local directory. These are fastq files contain reads that match the plastid genome.

To run the program in correction mode, use:

```bash
canu -correct genomeSize=160k -nanopore-raw *.fastq -p canu_corr -d ../canu_corr/
```

Where:
```bash
genomeSize # indicates the size of the genome of the organism to be analysed (in Mb or Gb, etc.)
-nanopore-raw # indicates what kind of inputs are being used
-p # prefix of output files
-d # output directory
```

To run canu in filter mode (_trim_), use:

```bash
canu -trim genomeSize=1.1g -nanopore-corrected /dir/corrected-files/fastq/PAD61315_fastq/file.fastq -p prefix_trim -d directory_output 
```

Where:
```bash
genomeSize # indicates the size of the genome of the organism to be analysed (in Mb - M or Gb - g, etc.)
-nanopore-corrected # indicates what type of inputs are being used
-p # prefix of the output files
-d # output directory
```

# Recommended literature
1. Koren S, Walenz BP, Berlin K, Miller J, Bergman NH, Phillippy A. (2017)  Canu: scalable and accurate long-read assembly via adaptive k-mer weighting and repeat separation. Genome Res. 27: 722-736
2. Sun J, Li R, Chen C, Sigwart JD, Kocot KM. (2021) Benchmarking ONT read assemblers for high-quality molluscan genomes. Proc. B Phil Trans. https://doi.org/10.1098/rstb.2020.0160
