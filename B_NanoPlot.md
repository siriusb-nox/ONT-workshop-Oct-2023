## B. DATA QUALITY ANALYSIS WITH `NanoPlot`
NanoPlot is a program designed to produce graphs and summaries of the results of sequencing experiments produced by ONT. The program receives as input a series of fastq files (using the `--fastq` option), or a text file produced by ONT (by default called "sequencing_summary.txt"). By parsing fastq files directly, you can produce graphs with more detailed information.

In this tutorial, we will work with the text file "sequencing_summary.txt" (available [here](https://drive.google.com/file/d/1dy6Sf3TZVkq7S0GOjyy8UlWolUeFEsxv/view?usp=share_link)): 

```bash
NanoPlot --summary sequencing_summary.txt --loglength --o summary-plots-log-transformed
```

As a result, the program will create a large number of plots in .png files plus a compilation of them in html format. These files will be placed in a folder called "summary-plots-log-transformed" (available [here](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NanoPlot/), gzipped folder).

An example of the output is the following:

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot.png" alt="Length vs Sequence quality"/>
</p>
