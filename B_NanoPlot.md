## B. DATA QUALITY ANALYSIS WITH `NanoPlot`
NanoPlot is a program designed to produce graphs and summaries of sequencing experiment results produced by ONT. The program uses as input a series of fastq files (using the `--fastq` option), or a text file produced by ONT (by default called "sequencing_summary.txt"). By parsing fastq files directly, you can produce graphs with more detailed information.

In this tutorial, we will work with the text file "sequencing_summary_FAT98192_deec7cb2_ec30cd82.txt.gz" (available [here](https://drive.google.com/file/d/1dy6Sf3TZVkq7S0GOjyy8UlWolUeFEsxv/view?usp=share_link)): 

```bash
NanoPlot --summary sequencing_summary.txt --loglength --o summary-plots-log-transformed
```

As a result, the program will create a large number of plots (.png files) plus a compilation in html format. These files will be placed in a folder called "summary-plots-log-transformed" (available [here]([https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NanoPlot/](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/NanoPlot/summary-plots-log-transformed.zip)), zipped folder).

An example of the output is the following plot, obtained from a previous sequencing experiment, using an R9 flowcell+chemistry (legacy):

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot.png" alt="Length vs Sequence quality"/>
</p>

This graph indicates how the DNA sequence length produced by ONT relates to their quality (expressed in values similar to [Phred]:(https://en.wikipedia.org/wiki/Phred_quality_score). Interestingly, many sequences have values below Phred 10 (i.e., probability of 1 in 10 nucleotides being called incorrectly). But how does this compare with the newest v14 kits and R10 flowcells? Here is a lenght vs sequence quality plot derived from the the sequencing experiment "Art_altilis_DRY_WGS17".  

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot_Art_altilis_DRY_WGS17.png" alt="Length vs Sequence quality"/>
</p>

Low quaility sequences will always happen, regardless of the sequencing technogloy. Incidentally, there are certain regions of the genome that are more difficult to sequence ([low complexity regions](https://academic.oup.com/nar/article/32/suppl_2/W628/1040725)), and ONT will tend to produce low sequence quality data from these regions (i.e., higher probability of miscalled bases, see **recommended literature** section) if such regions are associated with high GC content:

<p align="centre">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/GC_qual_bias_ONT_Delahaye_Nicolas_2021_PLoSONE.png" alt="GC content and quality bias in ONT data"/>
</p>

**Figure 4**. Global error rates of reads according to their GC content. GC contents were grouped by integer value. Only values supported by at least *n* reads were plotted. For each species the mean error rate is computed, and the median on all these values is displayed. A: Results for bacterial data, n = 100. Low-GC species have lower global error rates than high-GC ones (taken from Delahaye & Nicolas 2021, PLoSONE).

This other graph shows the number of sequences produced vs. their length:

<p align="centre">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/WeightedHistogramReadlength.png" alt="Length vs number of sequences"/>
</p>

Apart from the graphical output, `NanoPlot` generates a succinct report on the overall sequence, where values such as average and median sequence lengths, quality, number of reads etc. are provided. This file is usually called ``NanoStats.txt`` and looks like this:


```
General summary:         
Active channels:                    488.0
Mean read length:                 1,363.1
Mean read quality:                   13.0
Median read length:                 783.0
Median read quality:                 13.8
Number of reads:              4,488,678.0
Read length N50:                  2,330.0
STDEV read length:                1,727.3
Total bases:              6,118,383,425.0
Number, percentage and megabases of reads above quality cutoffs
>Q5:	4196018 (93.5%) 5827.1Mb
>Q7:	3944100 (87.9%) 5526.7Mb
>Q10:	3515763 (78.3%) 5019.4Mb
>Q12:	2986773 (66.5%) 4403.6Mb
>Q15:	1635361 (36.4%) 2634.3Mb
Top 5 highest mean basecall quality scores and their read lengths
1:	29.7 (2194)
2:	29.7 (2100)
3:	28.6 (279)
4:	28.1 (178)
5:	28.0 (464)
Top 5 longest reads and their mean basecall quality score
1:	317219 (3.7)
2:	262893 (3.6)
3:	234317 (3.8)
4:	213357 (3.8)
5:	194829 (4.1)

```

The full report of a sequencing experiment conducted using DNA from our mystery organism is available [here](https://drive.google.com/file/d/1jPEOJwQUAObTKwK9kZybFwkhUSoayszy/view?usp=share_link), or in the folder of this [repository](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NanoPlot) - file named "summary-plots-log-transformed.zip" (first unzip with `gunzip`). 

## Recomended literature
1. Delahaye C, Nicolas J. 2021. Sequencing DNA with nanopores: troubles and biases. PLoS ONE https://doi.org/10.1371/journal.pone.0257521
2. Orlov YL, Potapov VN. 2004. Complexity: an internet resource for analysis of DNA sequence complexity. Nuc. Ac. Res. 32 https://doi.org/10.1093%2Fnar%2Fgkh466
