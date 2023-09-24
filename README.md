# Workshop "Principles on the analysis of Oxford Nanopore Sequencing datasets" - 9-16 October 2023
## Instructors: [Oscar A. Pérez Escobar](https://operez-escobar.wixsite.com/orchidevo) [(Royal Botanic Gardens, Kew)](https://scholar.google.co.uk/citations?user=tSzyp6QAAAAJ&hl=en) & [Sidonie Bellot](https://www.kew.org/science/our-science/people/sidonie-bellot) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=KREJ2JsAAAAJ) 
## 
### Organizers: [Liam Trethowan](https://www.kew.org/science/our-science/people/liam-trethowan) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=FgqqcMMAAAAJ), Oscar A. Pérez Escobar, Sidonie Bellot.
##### This workshop is funded and supported by: [The Darwin Innitiative UK](https://www.darwininitiative.org.uk) - [Royal Botanic Gardens, Kew (RBG Kew)](https://www.kew.org) - [Herbarium Bogoriense-BRIN)](https://brin.go.id/en)

## 1. Introduction
This repository contains a tutorial guide to the analysis of raw data derived from Oxford Nanopore Technologies (ONT) and the initial steps for conducting a genome assembly. Additionally, it includes a real example of ONT data application/usability: e.g., conducting sequence searches on a predetermined database using ncbi blast to determine the identiy of an organism sequenced using ONT. The tutorial is in part based on data generated by Canales et al. (2022, article available [here](https://gigabytejournal.com/articles/71), using a [GridION](https://nanoporetech.com/products/gridion), which was used to assemble the nuclear genome of the cinchona tree (_Cinchona pubescens_, Rubiaceae). For the BLAST demonstrations, unpublished data from a mysterius organism will be used (data produced by Natalia Przelomska, Alexandre Antonelli, Diego Bogarín & Oscar A Pérez-Escobar).

This tutorial is intended for users with a basic knowledge in programming and is designed to run in UNIX environments. The participant should ideally have experience in the use of terminals, and text file management programs such as **awk, sed, grep, among others.**_ The workshop will be run on previously configured computers (Ubuntu 22.04). _A basic introduction to the UNIX enviroment with some useful commands is available [here](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/blob/main/bash_tutorial.md)_. 

This tutorial requires the following programs (dependencies) to run (it is highly recommended to have these programs installed before starting the tutorial). **Please make sure that the dependencies on which these programs run are also available**:

1. [**NCBI blast:**](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download) This program allows the construction of blast databases, and the search (alignment) of DNA or AA sequences (fasta format) in blast databases. 

2. [**NCBI magicblast:**](https://ncbi.github.io/magicblast/doc/download.html) This program allows the search of DNA sequences derived from massive sequencing (fasta or fastq format) in blast databases.

3. [**CANU:**](https://github.com/marbl/canu) this program allows the correction and filtering of ONT/PacBio sequences.  

4. [**SMARTdenovo:**](https://github.com/ruanjue/smartdenovo) this program assembles de-novo corrected and trimmed ONT/PacBio sequences.

5. [**NanoPlot:**](https://github.com/wdecoster/NanoPlot) An online executable version is available [here](https://nanoplot.bioinf.be/); this program produces plots with information associated with sequencing experiments conducted on ONT technologies.

6. [**Guppy:**](https://nanoporetech.com/nanopore-sequencing-data-analysis) This program is in charge of calling bases from FAST5 files generated by ONT. It is only available for ONT users (this part of the tutorial, although explained, will not be executed).
