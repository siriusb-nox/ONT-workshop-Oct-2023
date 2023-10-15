## C. ONT DATA CORRECTION AND FILTERING

There are several programs that can operate with ONT reads. In this workshop, we will rely on `canu`, which is a program designed to perform ONT read correction, filtering (removal of bases or fragments from sequences with low quality) and scaffold assembly. The program **requires a lot of resources for its execution** (this depends on the size of the genomes, their complexity and the initial amount of data being used for its execution. **The program recommends starting the run with an amount of data that represents between 20x-60x+ theoretical coverage (e.g., Sun et al., 2021).** The program can operate with lower ammounts of input data but at the expense of higher error rates (higher number of miscalled bases on the read after correction analysis).

*Excerpt from Sun et al.,2021* 
>The following assemblers were used for the benchmarking, including the long-read only assemblers (Canu [12], Flye [13], Wtdbg2 [14], Miniasm [15], NextDenovo >>(https://github.com/Nextomics/NextDenovo), NECAT [6], Raven [16] and Shasta [7]) and hybrid assemblers (MaSuRCA [17] and QuickMerge [18]). **Canu was not tested on the M. coruscus genome owing to the extremely intensive computing time required for this large genome**. Previous analyses have suggested that using corrected ONT reads could improve the genome assembly [19]. To check the effect that this has on the assemblies, the ONT reads that were corrected and/or trimmed by Canu.

`canu` is a modular program, this means that each mode (_correction_, _trim_, _assembly_) can be run separately. This is recommended as in some instances the program may crash due to lack of resources, especially when assembling complex genomes and using lots of input data (e.g., nuclear genomes of +1 Gb). In this case, it is recommended to use `canu` for corrections and filtering, and other programs for assembling corrected and filtered data (e.g. SMARTdenovo). 

In this section of the workshop, we will correct, trim and assemble a small subset of the _Artocarpus altilis_ dataset sequenced from a sample collected in a dry environment (WGS17). This will enable is to use `canu` for correction, trimming and assembly. We will aim to assemble the plastid genome of _A. altilis_!

The data required to run this section is available in the folder `NGSdat/fastq_subset/` of your local directory. These are fastq files contain reads that match the plastid genome.

To run the program in correction mode, use:

```bash
canu -correct genomeSize=160k -nanopore-raw *.fastq -p canu_corr -d ../canu_corr/
```

Where:
```bash
genomeSize # indicates the size of the genome of the organism to be analysed (in Mb, Gb, or Kb)
-nanopore-raw # indicates what kind of inputs are being used
-p # prefix of output files
-d # output directory
```

We will use here a genome size of 160,000 bp because is the standard size of a plastid genome (also, a plastid genome of reference of A. altilis, available in [GenBank](https://www.ncbi.nlm.nih.gov/nucleotide/NC_059002.1), indicates that it has 160,184 bp).

This command will output reads that have been corrected. Correction occurres by building overlap graphics (see Fig. 2 of this [paper](https://genome.cshlp.org/content/27/5/722/F2.expansion.html)). This commmand will also issue a report that looks like this:

```bash
[CORRECTION/LAYOUT]
--                             original      original
--                            raw reads     raw reads
--   category                w/overlaps  w/o/overlaps
--   -------------------- ------------- -------------
--   Number of Reads               6030           544
--   Number of Bases           33156409       1831339
--   Coverage                   207.228        11.446
--   Median                        4152          2563
--   Mean                          5498          3366
--   N50                           6467          4381
--   Minimum                       1001             0
--   Maximum                      62276         41616
--   
--                                        --------corrected---------  ----------rescued----------
--                             evidence                     expected                     expected
--   category                     reads            raw     corrected            raw     corrected
--   -------------------- -------------  ------------- -------------  ------------- -------------
--   Number of Reads               6026            491           491            133           133
--   Number of Bases           31502614        6839053       6400196        1090108        291738
--   Coverage                   196.891         42.744        40.001          6.813         1.823
--   Median                        4036          11603         11237           5348          1914
--   Mean                          5227          13928         13035           8196          2193
--   N50                           6056          13582         12560          12933          2395
--   Minimum                       1001           8831          8794           1523          1036
--   Maximum                      56138          56138         38973          33858          6024
--   
--                        --------uncorrected--------
--                                           expected
--   category                       raw     corrected
--   -------------------- ------------- -------------
--   Number of Reads               5950          5950
--   Number of Bases           27058587      14425376
--   Coverage                   169.116        90.159
--   Median                        3787          1928
--   Mean                          4547          2424
--   N50                           5166          4992
--   Minimum                          0             0
--   Maximum                      62276         48522
--   
--   Maximum Memory          1083529106
```


To run canu in filter mode (_trim_), use:

```bash
canu -trim genomeSize=160k -nanopore-corrected canu_corr.correctedReads.fasta.gz -p canu_trim -d ../canu_trim/
```

Where:
```bash
-nanopore-corrected # indicates what type of inputs are being used
-p # prefix of the output files
-d # output directory
```

This command will output reads that have been trimmed. Correction occurres by building overlap graphics.

```bash
canu -assemble -p A_altilis_CP genomeSize=160k correctedErrorRate=0.14 -nanopore-corrected canu_trim.trimmedReads.fasta.gz -d ../canu_ass/
```



# Recommended literature
1. Koren S, Walenz BP, Berlin K, Miller J, Bergman NH, Phillippy A. (2017)  Canu: scalable and accurate long-read assembly via adaptive k-mer weighting and repeat separation. Genome Res. 27: 722-736
2. Sun J, Li R, Chen C, Sigwart JD, Kocot KM. (2021) Benchmarking ONT read assemblers for high-quality molluscan genomes. Proc. B Phil Trans. https://doi.org/10.1098/rstb.2020.0160
