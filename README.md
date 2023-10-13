# Workshop "Principles on the analysis of Oxford Nanopore Sequencing datasets" - 9-16 October 2023
## Instructors: [Oscar A. Pérez Escobar](https://operez-escobar.wixsite.com/orchidevo) [(Royal Botanic Gardens, Kew)](https://scholar.google.co.uk/citations?user=tSzyp6QAAAAJ&hl=en) & [Sidonie Bellot](https://www.kew.org/science/our-science/people/sidonie-bellot) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=KREJ2JsAAAAJ) 
## 
### Organizers: [Liam Trethowan](https://www.kew.org/science/our-science/people/liam-trethowan) [(Royal Botanic Gardens, Kew)](https://scholar.google.com/citations?user=FgqqcMMAAAAJ), Oscar A. Pérez Escobar, Sidonie Bellot.
##### This workshop is funded and supported by: [The Darwin Innitiative UK](https://www.darwininitiative.org.uk) - [Royal Botanic Gardens, Kew (RBG Kew)](https://www.kew.org) - [Herbarium Bogoriense-BRIN)](https://brin.go.id/en)

## 1. Introduction
This repository contains a tutorial guide to the analysis of raw data derived from Oxford Nanopore Technologies (ONT) and the initial steps for conducting a genome assembly. Additionally, it includes a real example of ONT data application/usability e.g., conducting sequence searches on a predetermined database using ncbi blast to determine the identiy of an organism sequenced using ONT. The tutorial is partly based on data generated by Canales et al. (2022, article available [here](https://gigabytejournal.com/articles/71), using a [GridION](https://nanoporetech.com/products/gridion). The data was used to assemble the nuclear genome of the fever tree (_Cinchona pubescens_, Rubiaceae). For the BLAST analysis, unpublished data from a mysterius organism will be used (data produced by Natalia Przelomska, Alexandre Antonelli, Diego Bogarín & Oscar A Pérez-Escobar).

This tutorial is intended for users with a basic knowledge in programming and is designed to run in UNIX environments. The participant should ideally have experience using shell, and text file manipulation (e.g., using **awk, sed, grep, among others**). The workshop will be run on pre-configured laptops (Ubuntu 22.04). _A basic introduction to the UNIX enviroment with some useful commands is available [here](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/blob/main/bash_tutorial.md)_. 

This tutorial requires the following programs/dependencies (it is highly recommended to have these installed before starting the tutorial). **Please make sure that the dependencies on which these programs run are also available**:

1. [**NCBI blast:**](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download) This program builds blast databases, which are required for searches of DNA/AA sequences in blast databases. 

2. [**NCBI magicblast:**](https://ncbi.github.io/magicblast/doc/download.html) This program conducts DNA/AA sequence searches derived from illumina/Nanopore sequencing experioments (in fasta or fastq format) against blast databases.

3. [**CANU:**](https://github.com/marbl/canu) this program allows the correction and filtering of ONT/PacBio sequences.  

4. [**SMARTdenovo:**](https://github.com/ruanjue/smartdenovo) this program assembles de-novo corrected and trimmed ONT/PacBio sequences.

5. [**NanoPlot:**](https://github.com/wdecoster/NanoPlot) An online executable version is available [here](https://nanoplot.bioinf.be/); this program produces plots with information associated with sequencing experiments conducted on ONT technologies.

6. [**Guppy:**](https://nanoporetech.com/nanopore-sequencing-data-analysis) This program calls bases from FAST5 files generated by ONT. It is only available for ONT users (this part of the tutorial, although explained, will not be executed).


## 2. Workshope structure
This tutorial is divided into four main steps:

A. [**Base Calling**](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/A_basecall.md)

B. [**Data quality analysis**](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/B_NanoPlot.md)

C. [**Data trimming and correction**](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/C_read_corrtrim_CANU.md)

D. [**Genome search and/or assembly operations**](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/blob/main/D_blast.md)

![Figure 1](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/blob/main/IMG/pipeline_overview_v0_OP_15122022.png?raw=true)
**Figure 1**: Simplified view of tutorial/pipeline

**IMPORTANT:** The base data needed to run this tutorial is available at the "NGS" and "NanoPlot" folders of this repo.

## 2.1. Pipeline configuration
In any bioinformatics pipeline, it is essential to relate which programs the pipeline depends on and to know where the input files, etc. are located. To run this tutorial, you must copy this repository to a directory of your choice. To do this, please execute:

`git clone https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022.git`

**For users with programs installed in a UNIX environment on personal computers**, these can be entered in the current session (terminal) using the following command, for example:

`PATH=$PATH:/directory/of/the/folder/programax`

For this particular workshop, users with Dell Laptops should run the following lines to add the dependencies to ENV:

```bash
# Canu
PATH=$PATH:/home/ontasia*/Softwares/canu/canu-1.9/Linux-amd64/bin/
# Racon 
PATH=$PATH:/home/ontasia*/softwares/genomics/racon/build/bin
# Minimap2
PATH=$PATH:/home/ontasia*/softwares/genomics/minimap2-2.17_x64-linux/
# samtools
PATH=$PATH:/home/ontasia*/softwares/genomics/samtools-1.10
# magicblast
PATH=$PATH:/home/ontasia*/softwares/genomics/ncbi-magicblast-1.5.0/bin/
# ncbi blast
PATH=$PATH:/home/ontasia*/softwares/genomics/ncbi-blast-2.10.0+/bin/
# SMARTdenovo
PATH=$PATH:/home/ontasia*/softwares/genomics/
export PATH
```

## ACKNOWLEDGEMENTS
Natalia Przelomska and Alexandre Antonelli produced unpublished data. Ilia Leitch procured ONT sequence data for Cinchona.
