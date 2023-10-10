## B. DATA QUALITY ANALYSIS WITH `NanoPlot`
NanoPlot is a program designed to produce graphs and summaries of sequencing experiment results produced by ONT. The program uses as input a series of fastq files (using the `--fastq` option), or a text file produced by ONT (by default called "sequencing_summary.txt"). By parsing fastq files directly, you can produce graphs with more detailed information.

In this tutorial, we will work with the text file "sequencing_summary.txt" (available [here](https://drive.google.com/file/d/1dy6Sf3TZVkq7S0GOjyy8UlWolUeFEsxv/view?usp=share_link)): 

```bash
NanoPlot --summary sequencing_summary.txt --loglength --o summary-plots-log-transformed
```

As a result, the program will create a large number of plots (.png files) plus a compilation in html format. These files will be placed in a folder called "summary-plots-log-transformed" (available [here](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NanoPlot/), gzipped folder).

An example of the output is the following:

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot.png" alt="Length vs Sequence quality"/>
</p>

This graph indicates how the DNA sequence length produced by ONT relates to their quality (expressed in values similar to [Phred]:(https://en.wikipedia.org/wiki/Phred_quality_score)). Interestingly, many sequences have values below Phred 10 (i.e., prob 1 in 10 nucleotides of being called incorrectly). Incidentally, there are certain regions of the genome that are more difficult to sequence ([low complexity regions](https://academic.oup.com/nar/article/32/suppl_2/W628/1040725)), and ONT will tend to produce sequence data from these regions with lower qualities (i.e., higher probability of miscalled bases, see **recommended literature** section) if such regions are associated with high GC content:

<p align="centre">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/GC_qual_bias_ONT_Delahaye_Nicolas_2021_PLoSONE.png" alt="GC content and quality bias in ONT data"/>
</p>

**Figure 4**. Global error rates of reads according to their GC content. GC contents were grouped by integer value. Only values supported by at least *n* reads were plotted. For each species the mean error rate is computed, and the median on all these values is displayed. A: Results for bacterial data, n = 100. Low-GC species have lower global error rates than high-GC ones (taken from Delahaye & Nicolas 2021, PLoSONE).
