## C.1. ONT DATA CORRECTION AND FILTERING

There are several programs that can operate with ONT reads. In this workshop, we will rely on `canu`, which is a program designed to perform ONT read correction, filtering (removal of bases or fragments from sequences with low quality) and scaffold assembly. The program **requires a lot of resources for its execution** (this depends on the size of the genomes, their complexity and the initial amount of data being used for its execution. **The program recommends starting the run with an amount of data that represents between 20x-60x+ theoretical coverage (e.g., Sun et al., 2021).** The program can operate with lower ammounts of input data but at the expense of higher error rates (higher number of miscalled bases on the read after correction analysis).

*Excerpt from Sun et al.,2021* 
>The following assemblers were used for the benchmarking, including the long-read only assemblers (Canu [12], Flye [13], Wtdbg2 [14], Miniasm [15], NextDenovo >>(https://github.com/Nextomics/NextDenovo), NECAT [6], Raven [16] and Shasta [7]) and hybrid assemblers (MaSuRCA [17] and QuickMerge [18]). **Canu was not tested on the M. coruscus genome owing to the extremely intensive computing time required for this large genome**. Previous analyses have suggested that using corrected ONT reads could improve the genome assembly [19]. To check the effect that this has on the assemblies, the ONT reads that were corrected and/or trimmed by Canu.

`canu` is a modular program, this means that each mode (_correction_, _trim_, _assembly_) can be run separately. This is recommended as in some instances the program may crash due to lack of resources, especially when assembling complex genomes and using lots of input data (e.g., nuclear genomes of +1 Gb). In this case, it is recommended to use `canu` for corrections and filtering, and other programs for assembling corrected and filtered data (e.g. SMARTdenovo). 

In this section of the workshop, we will correct, trim and assemble a small subset of the _Artocarpus altilis_ dataset sequenced from a sample collected in a dry environment (WGS17). This will enable is to use `canu` for correction, trimming and assembly. We will aim to assemble the plastid genome of _A. altilis_!


>[!IMPORTANT]
>The data required to run this section should be made available in the folder `NGSdat/fastq_subset/` of your local directory. These are `*.fastq` files contain reads that match the plastid genome. These files are available [here](https://drive.google.com/file/d/1om15NZPe6w4jbHM62FbrrkTNBU_3-3pc/view?usp=sharing). Make sure to download it and store in the `NGSdat/canu_fastq` directory in your local machine (have a look at the `README.md` file in the folder `canu_fastq` folder of this repo).

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

This command will output reads that have been trimmed. 

## C.2. ONT DATA ANALYSIS: WHOLE GENOME ASSEMBLY (WGA)

Finally, assembling genomes with corrected ONT data can be more or less staright forward depending on the complexity of the genome (ploidy levels, proportion of repetitive elements).

To assemble the plastid genome using `canu` and trimmed reads, try the following command:

```bash
canu -assemble -p A_altilis_CP genomeSize=160k correctedErrorRate=0.14 -nanopore-corrected canu_trim.trimmedReads.fasta.gz -d ../canu_ass/
```

Where:
```bash
-nanopore-corrected # indicates what type of inputs are being used
correctedErrorRate=0.14 # indicates the overalp error rate. The value here is adequate for corrected nanopore reads.
-d # output directory
```

In some instances, using `canu` for genome assembly might become prohibited when lots of input data are being used, or complex, large genomes are being assembled. In such case, a good alternative for conducting genome assembly is `SMARTdenovo`. This program runs considerably much faster than `canu`, with a [comparable performance](https://gigabytejournal.com/articles/15). It requires as input data raw or corrected ONT reads.

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/Liu_al_2023_Gigabyte_SMARTdenovo.png" alt="Performance of SMARTdenovo"/>
</p>

To run `SMARTdenovo`, use the following command.

```bash
smartdenovo.pl -c 1 -t 64 -p prefix_output *fastq.fastq > prefix_output.mak

make -f *.mak
```

Where:
```bash
-p # output prefix 
-t # number of threads (default is 8)
-k # k-mer length for overlapping (default is 16)
-J # min read length (default is 5000)
-c # generate consensus sequence
```

### ACTIVITY
1. Attempt to assemble a draft genome from the trimmed `canu` reads, using SMARTdenovo. How different is the resulting genome assemnbly from that produced by `canu`?

## C.3. ONT DATA ANALYSIS: GENOME POLISHING
When assembling genomes with ONT data, it is extremely important to ensure that the scaffolds are as accurate as possible. Here, erroneously sequenced based from ONT data might have remained could prevent better contiguity in the assembly. One way to improve accuracy is by "polishing" the genome, using short read sequencing data produced from the **exact same individual**. This is done in two steps. 

1. Mapping the illumina sequencing data against the genome assembly, using read aligners, like `minimap2`. This programs generates an index files with coordinates of the reads mapped against the genome assembly. Here, every read has a quality mapping score, and information on on when bases from the short reads disagree with the reference. THe ouput of this command is a _SAM_ file (read [here](https://en.wikipedia.org/wiki/SAM_(file_format)) more about SAM files). **In this workshop, We wont run these commands because we lack the illumina sequencing data**. To run `minimap2`, execute:

```bash
minimap2 -ax sr -t 64 ../genome.assembly.fasta input_illumina_read_files_R1_001.fastq input_illumina_read_files_R2_002.fastq > output_minimap.sam
```

Where:

```bash
-ax # map short reads. 
-t # number of threads
```

2. Correct the scaffolds using the SAM file produced by `minimap2`, and the ONT filtered and corrected reads (produced by `canu`). We use `racon` to this end.

```bash
racon -t 64 corrected.trimmed.canu.reads.fasta output_minimap.sam genome.assembly.smartdenovo.fasta
```

Where:

```bash
-t # number of threads
```

# Recommended literature
1. Koren S, Walenz BP, Berlin K, Miller J, Bergman NH, Phillippy A. (2017)  Canu: scalable and accurate long-read assembly via adaptive k-mer weighting and repeat separation. Genome Res. 27: 722-736
2. Sun J, Li R, Chen C, Sigwart JD, Kocot KM. (2021) Benchmarking ONT read assemblers for high-quality molluscan genomes. Proc. B Phil Trans. https://doi.org/10.1098/rstb.2020.0160
3. Liu H, Wu S, Li A, Ruan J. (2021). SMARTdenovo: a de-novo assembler using long noisy reads. Gigabyte. https://gigabytejournal.com/articles/15
4. Vaser R, Sovic I, Nagarajan N, Sikic M. (2017). Fast and accurate de novo genome assembly from long uncorrected reads. Genome Res. https://genome.cshlp.org/content/early/2017/01/18/gr.214270.116
5.  Li H. (2018). Minimap2: pairwise alignment for nucleotide sequences. Bioinformatics, 34:3094-3100. https://academic.oup.com/bioinformatics/article/34/18/3094/4994778?login=true

