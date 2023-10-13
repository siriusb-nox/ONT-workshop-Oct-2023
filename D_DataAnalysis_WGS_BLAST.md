## ONT DATA ANALYSIS: BLAST AND WHOLE GENOME ASSEMBLY (WGA)

In this last section, we will demonstrate how to use ONT data (post correction/filtering). Perhaps one of the most widely used applications of ONT data is the identification of organisms by alignment methods, e.g. using [BLAST](https://www.ncbi.nlm.nih.gov/books/NBK279690/). In the following exercise, we will query a subset of our sukun sequences against a predetermined dataset. To do this we will run i) a local search by creating database in `makebklastdb` and `magicblast`.The database we will create will use as input plastid genome data of sukun. The idea here is to assess which proportion of our ONT data matches the plastid DNA. Examples of commands:

```bash
makeblastdb -in refgenomes_org.test.fasta -out refgenomes_org.test_refDB -parse_seqids -dbtype nucl

magicblast -query archivo_fastq -db refgenomes_org.test_refDB -out ab.out -outfmt tabular -no_unaligned -infmt fastq
```
