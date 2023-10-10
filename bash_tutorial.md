<p align="center">
  <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/bash_logo_bashlogo.com.png" alt="bash logo from bash webpage"/>
</p>
  
## Basic tutorial for navigating a terminal in UNIX environments

#### 1. Introduction
UNIX is a family of operating systems (OS) developed in 1960. Perhaps the most famous OSs are [Ubuntu](https://ubuntu.com/), [Linux](https://www.linux.org/) and [MacOS](https://www.apple.com/uk/macos/ventura/). In bioinformatics, _bash_ (i.e., "bourne Again Shell") or the _shell_ (i.e., terminal, or the translator between the operations the computer performs and the "human" language instructions) of UNIX is a widely used language as it contains a number of basic but powerful programs for handling text files (e.g., fasta sequences, fastq, genomes, etc.). Additionally, many programs in bioinformatics are more available for UNIX environments than other platforms (e.g. Microsoft's DOS... urgh!). 

#### 2. Syntax
UNIX syntax in the terminal is very easy. Any time the terminal displays "$" or "%", the terminal is ready to receive instructions.  Basically:

`$program option1 option2 input > output`.

Options in UNIX are usually parameters (_flags_) that allow you to modify the usual/default behaviour of a program. They are usually specified with a "-", for example: 

`ls` vs `ls -lrt` - What is the difference between the two commands?

#### 3. Essential programs for navigating the UNIX terminal and interacting/displaying files
Here is a short list of basic UNIX programs that we will use often:

#### 3.1 `cd`

"Change directory" is used to change from one folder/directory to another. For example:

```bash
cd
```
To move a folder up, use "cd ..". In addition, _cd_ will also folder a complete directory address, e.g. **/home/usr/etc**:

```bash
cd ..
cd /home/usr/etc
```

### 3.2 `mkdir`
Creates a new folders.  

```bash
mkdir foldernew
```
Several folders can be created at the same time, e.g:

```bash
mkdir folder1 folder2 folder3 folder3
```
Entire directories can be created with the _-p_ option, like this:

```bash 
mkdir -p /home/usr/usr/foo/bla/
```
Folders can also be created in existing subdirectories, using an existing address. For example, create 'bioinf' in '/home/usr/foo/bla/':

```bash 
mkdir /home/usr/usr/foo/bla/bioinf
```

### 3.3 ``pwd``
Displays the directory you are in

```bash
pwd
```

### 3.4 `ls`
Lists the files or folders present in a directory. `ls` has a lot of options... For example, the `-l` option presents a detailed list of files/folders, showing info such as exact file size, owner, access, and modification date.

```bash
ls
ls -l
```

### 3.5 ``cat``
Used to view "non-binary" files, e.g.: 
* View text files in the terminal
* Merge text files  
 
```bash
cat file.txt
cat file1.txt file2.txt
cat file1.txt file2.txt > filecombinedfile.txt
cat < file1.txt > file2.txt
```

### 3.6 ``less`.
Used for browsing ``non-binary`` files, e.g.: 

```bash
less input.txt
```
For example, ``less` can be used to take a quick look at a fastq file, like this (file available in the tutorial, first unzip using `gunzip`):

```bash
cat /directory/staff/Workshop-Oxford-Nanopore-Dec-2022/NGSdat/Cinchona_PAD61320_sizeSelect_1Kseq_99.fastq
```

### 3.7 ``grep``
This program searches for text patterns, including [_regular expressions_](https://sospedia.net/el-shell-bash-de-gnulinux-4-expresiones-regulares/). 

```bash
grep option1 option2 input
```

For example, we can search for a specific sequence name ("@7318713f-55b4-4e19-8fb1-4bd89b35568e") in a fastq file like this: 

```bash
grep "^@7318713f-55b4-4e19-8fb1-4bd89b35568e" /directorio/personal/Taller-Oxford-Nanopore-Dec-2022/NGSdat/NGSdat/Cinchona_PAD61320_sizeSelect_1Kseq_99.fastq
```

... there are many other programs that allow us to visualise data that we recommend learning about. Some examples are available [here](https://www.biostars.org/p/17680/).

### 3.8 `man`.
This program provides detailed information about basic UNIX programs.

```bash
man program
man ls
```
