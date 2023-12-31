## D.1. ONT DATA ANALYSIS: BLAST 

In this last section, we will demonstrate real applications of ONT data in genomics: _A_) querying sequences (ONT reads) againsts a reference, and _B_) assemblying draft genomes.  

Perhaps one of the most widely used applications of ONT data is the identification of organisms by alignment methods, e.g. using [BLAST](https://www.ncbi.nlm.nih.gov/books/NBK279690/). Blast takes as input nucleotide or aminoacid seaquences and aligns them against a database of sequences. This database needs to be created through one of the applications of blast, `makeblastdb`. The alingment (search) is then conducted (depending on the input data) by `blastn`, `blastp`, or `magicblast`.

### ACTIVITY:
To find out which proportion of the ONT data does correspond to _Artocarpus_ DNA, in the following exercise, we will query our ONT reads against the reference nuclear genome of _Artocarpus heterophyllus_. **NB: The files needed to run this section of the workshop are available in the folder [NGSdat](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NGSdat) of this repository.**

To this end, you will: 

1. Download the reference genome of _A. heterophyllus_ from the NCBI webpage (available [here:](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_025403435.1/)).
2. Construct locally a blast database from this reference genome. The basic command is:

```bash
makeblastdb -in refgenomes.fasta -out refDB.out -parse_seqids -dbtype nucl
```

where:

```bash
-in # your input reference genome (fasta format)
-out # the name of your outoput database
-parse_seqids # keep the origina IDs of the sequences in the reference genome
-dbtype # specify the type of database to be constructed (nucleotide, protein)
```

3.a. Align the ONT reads against the local blast database created on step 2, using `magicblast` (unlike `blastn`, this program can use *fastq input files). An example of a command is the following: 

```
magicblast -query input_ONT.fastq -db refDB.out -out blast.search.out -outfmt tabular -no_unaligned -infmt fastq
```

where:

```bash
-query # your input ONT reads (fastq format)
-db # your blast database (created on step 2)
-out # your output namefile
-outfmt # output format (table, SAM file)
-infmt # type of input (fastq, fasta)
```

The output should be a tab delimited file, and will look like this.

```bash
# MAGICBLAST 1.7.2
# magicblast -query FAT98192_pass_deec7cb2_ec30cd82_9.fastq -db ../../../blastDB/Art_altilis_CP_NC_059002.1.blastdb -out /Users/o.perez-escobar/Documents/JORMUNGANDR/Projects/ONT_DarwinBogor_2023/GitHub/ONT_JAVA_2023/ONT-workshop-Oct-2023/NGSdat/magicblast_out/FAT98192_pass_deec7cb2_ec30cd82_9.out -outfmt tabular -no_unaligned -infmt fastq 
# Fields: query acc.    reference acc.  % identity      not used        not used        not used        query start     query end       reference start reference end   not used        not used        score   query strand reference strand        query length    BTOP    num placements  not used        compartment     left overhang   right overhang  mate reference  mate ref. start composite score
68dc22d3-44d6-593f-9dd1-e55f86169c77    NC_059002.1     99.2769 0       0       0       35      1414    118851  120231  0       99      1333    plus    plus    1415    82-T-A339A-711AG2AGTC3-T64TAC-168CT4    1            -       1:0     GCAATACGTAACTGAACCAAGTACAGGCAA  T       -       -       1333
8e298d68-42ba-4ee1-9641-c39f4bc490a1    NC_059002.1     74.7208 0       0       0       28      1004    55340   54373   0       99      572     plus    minus   13962   61G-26GA1TG2-A2GA27C-G-1TA2CA26GC3C-35A-21-A16-T-A8T-T-G-1CA18AG2CAAG1-A7AT1T-18_216_%210%106CT62-G87-A31CAAG126C-T-19-T26   1       -       1:1     AGCAATACGTAACTGAACGAAGCCACA     CCTCGTGTCCAAAGTATGAAGATTTCCCTA  -       -       572
```

**Try now to code a _for_ loop to blast all the files in one like of code. Then, using the `magicblast` output, try to find out how many fastsq sequences match the genome of A. heterophyllus!** You might want to use `awk`, `sort`, `cut`, `sed` and/or `wc` to do this task.
<!-- [!WARNING] >![Figure 1](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/Screenshot%202023-10-19%20at%2010.15.16.png?raw=true) -->

3.b. Align the ONT reads, using NCBI remote services. To this end, we need to rely on the blast tool of NCBI, because is the only capable of searching across multiple databases. The *fastq reads need to be converted first to fasta format. To do this, try the following.

```bash
cat input.fastq | paste - - - - | cut -f 1,2 | sed 's/@/>/g' | tr -s "/t" "/n" > output.fasta
```

>[!NOTE]
>**In this workshop, we will work with a subset of fasta reads derived from our sequence data of A. altilis WGS17 sequencing experiment.** The file is available in the [NGSdat folder](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/NGSdat/A_altilis_CP.unitigs.fasta) of this repository.

We will try to find out how many of the ONT read data does mat NCBI remote search againts the "nu" database. Here, is important to have access to an NCBI account and an API key. Using an API key will enable higher number of sequence searches per second agains the online databases. To introduce your API key in PATH, use:

```bash
export NCBI_API_KEY=09ddca88b438c887b83f8d58fcc890c321XX
```

To execute blast in remote mode, use: 

```bash
blastn -query FAT98192_pass_deec7cb2_ec30cd82_9.fasta -db nt -remote -task blastn-short -evalue 0.01 -entrez_query "Artocarpus [organism]" -outfmt 6 -out blast_result_misteriousplant.table -max_target_seqs 10 -max_hsps 5
```

where:

```bash
-query # your input ONT reads (fasta format)
-remote # conduct an online search remotely
-task # optimise for a particular analysis (here short read data, is faster)
-evalue # value to exclude poor sequence matches
-db # your blast database (created on step 2)
-out # your output namefile
-outfmt # output format (table)
-max_target_seqs # Number of aligned sequences to keep.
-max_hsps # Maximum number of HSPs (alignments) to keep for any single query-subject pair.
```


The output of this analysis will look lije this:

```
93c9b0d6-2802-485b-8998-435462f77771	MW375124.1	93.294	1178	39	13	34	1190	71181	70023	0.0	1659
93c9b0d6-2802-485b-8998-435462f77771	MW375125.1	93.248	1170	48	12	34	1190	71757	70606	0.0	1635
93c9b0d6-2802-485b-8998-435462f77771	NC_043905.1	93.469	1133	42	12	76	1190	70163	69045	0.0	1604
93c9b0d6-2802-485b-8998-435462f77771	MW375128.1	92.760	1174	53	12	34	1189	72076	70917	0.0	1598
93c9b0d6-2802-485b-8998-435462f77771	MW375126.1	93.412	1108	47	11	101	1190	71407	70308	0.0	1560
231acd9d-2063-4509-9ccc-c822ff4f66d7	MF861028.1	94.057	774	23	6	33	799	758	1	0.0	1156
231acd9d-2063-4509-9ccc-c822ff4f66d7	MF861030.1	93.928	774	23	7	33	799	757	1	0.0	1140
231acd9d-2063-4509-9ccc-c822ff4f66d7	MF861029.1	93.928	774	23	7	33	799	757	1	0.0	1140
231acd9d-2063-4509-9ccc-c822ff4f66d7	MF860862.1	94.802	731	22	5	33	756	737	16	0.0	1130
231acd9d-2063-4509-9ccc-c822ff4f66d7	MF860864.1	94.665	731	22	6	33	756	736	16	0.0	1114
0d3193bd-6dca-4f57-bf00-665e9cc55904	MW375130.1	97.489	1314	20	7	30	1339	36038	37342	0.0	2300
```

## D.1. ONT DATA ANALYSIS: PLASTID GENOME ANNOTATION
Genome annotation is the process of obtaining functions and structures of a genome. It is an essential step towards the completion of a genome assembly because assemblies come without such information. 

One of the most popular tools for organellar genome annotation is `GetSeq` from the [CHLOROBOX tool kit](https://chlorobox.mpimp-golm.mpg.de). The annotation occurs by comparing a submitted genome assembly with those available in the NCBI database and curated by `GetSeq`. In this last section of the workshop, we will learn how to annotate our own plastid genome assembly using `GetSeq`.

### ACTIVITY
1. Have a good look at the assembled scaffolds produced by `canu` (it should be a file called `prefix.unitigs.fasta`, e.g., `A_altilis_CP.unitigs.fasta`. **Find out how many scaffolds there are.** Is there any scaffold that matches the known genome size of a plastome? (hint: use `grep`for this task).
2. Try to extract from this file the sequence that matches in size the size of a regular plastome (~160,000 bp). To this end, you would like to first convert your fasta file from interleave (DNA sequence in multiple lines) to sequential (DNA seq in a single line). Use the following command to do so.

```bash
awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' input.interleaved.fasta > output.singleline.fasta
```

3. This sequence is ready to be uploaded into the [GetSeq](https://chlorobox.mpimp-golm.mpg.de/geseq.html) webpage.

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/getseq_chlorobox_screenshot.png" alt="A screenshot of the GetSeq web page"/>
</p>




