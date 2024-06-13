# Welcome to IGV
This document is going to go serve as an introduction to IGV, including how to set it up, the major features of the program, and how to idenify differrent variants, including single nucleotide variants (SNVs), small variants from 2-50 bases (indels), and larger structural variants of at least 50 bases (SVs). There will also be examples and questions along the way to help this information sink in. 

For this tutorials a few peices of setup are required. First, the desktop apllications for nIGV can be downloaded [here](https://igv.org/doc/desktop/#DownloadPage/). You will have to select the right version of the application for your computer (Mac, PC, etc). Second, we will be using various files throughout this tutorial. These files can be found at `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial`. These files are
- proband bam
- father bam
- mother bam
- `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial/HG005_SNVs_CHR1.vcf` and `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial/HG005_SNVs_CHR1.vcf.idx`: The `.vcf` file contains a list of SNVs present in the first chromosme of HG005. the `.idx` file is an index file for the `.vcf` file. In IGV if the `.vcf` file is too large it uses an index file top help loads the data
- list of SVs in the proband
  
After conferming you have access to these files and have downloaded IGV continue on to the rest of the tutorial

# What is IGV
IGV, also know as the Integrative Genomics Viewer, is a tool developed by the broad (Robinson JT, Thorvaldsdóttir H, Winckler W, Guttman M, Lander ES, Getz G, Mesirov JP. Integrative genomics viewer. Nat Biotechnol. 2011 Jan;29(1):24-6. doi: 10.1038/nbt.1754. PMID: 21221095; PMCID: PMC3346182.) The goal of IGV is to allow users to manually viulaize different regions of a given patients genome. The power of this comes from if you know what reference contig a genome is aligned to and what a varisnt would look like in IGV, you can conferm weather a variant called by some variant caller is a true positive for a false positive. This cna greatly increase the sensetiviy and specificty of a given pipeline, especially those which are known to have high false positive rates. Keep in mind though IGV require manual labour, making iot difficult to visulize a large number of variants. The amount of variants possible to be visuzlied in a given time frame depends on the variants being looked at.

# Setting up IGV
After opening IGV you should be greeted to a screen that looks similar to this

![Screenshot of empty igv window](/Images/Empty_IGV_Image.png)

There are a few key spots to point out
- Reference Contig: This is the reference contig what will be shown. It is important to make sure this matches the contig the bam files you intend to visualzie was aligned to
- current chromosme: this will tell you the curent cromosme your window shows. Keep in mind dpend on the config it could have many more options that the standard 24 chomosome
- current region: This shows you the exact coordinates of the window you are looking at in the form `chr#:start-end`. In addition, if there is a specific region you want to look at you can input the cordinates in the same format and click 'Go'. You can also search a specific gene and it will shou you the cordinates of that gene.
- Zoom in and Zoom out: This will decrease or increase the size of the window respecitivly. Fairly self explanatory

There are a few setting you may want to adjust before visualziing any variants. To adjust the settings go to `View->Preferences` The majority of the setting you will want to adjust are under `Alignments`
- `Color aligments by`: This controls the color of each read, which doesn't really matter unless you are visualziing SVs (that is gonna be a common trend), but recomended to have it set to `INSERT_SIZE`
- `Group alignments by`: This will group reads of the same atribute together. Again only really matters for SVs, in which case have it set to `PAIR_ORIENTATION`
- `Mapping quality threshold`: The mapping quality of a reads is −10*log<sub>10</sub>Pr(mapping position is wrong). This threshould will remove all reads with a mapping quality below it. Thus the number is larger the more acuate it is. We recomend setting the threshold to 1 to remove all reads where there is no confidence in which it is mapped to (you should not base any analysis on these reads)
- `Aligment display mode`: controls how the reads look like. setting it to `expland` make the reads as large as possible, which make them easier to interperate 
- `Show soft mismatched bases` and `show soft-clipped bases`: controls if you see bases that don't match the reference. both should be checked as we are looking for devitations from the reference sequence

With these settings you are already to begin visualizeing bam files

## loading files and sessions
Go to `File->Load from File` to load a file into IGV. There are two main types of files you will want to load
- Genomic sequencing files: These will be files with the actual sequecning of a pateints geneome. In almost all cases you will be loading a `.bam` or a `.cram` file. Of note for IGV to be able to load the file you must have the coresponding index file (either a `.bai` or `.crai`) in the same folder
- Variant list file: This file will have the list of vairants you want to visualzie, likely either a list of SNVs, Indels, or SVs. This could either be a `.vcf` or a `.bed` file.
If there is any other file you have stored locally or on the RIS that you think will help with your analysis (for example I have loaded encode data to see if any strucutal variants overlap with a known encoder), you can load it using this same method

There are a few addition files IGV provides that you may want to load for you analyis, for example list of know repeditive regions or list of known genes. To load these files go to `File->Load from Server`, Check the boxes for the files you want to load and click `OK` to load them. 

### CHALANGE 1
Can you make your IGV look the same as the following screenshot

<details><summary>HINT 1</summary>
&emsp; &ensp; Repeat masker, which is a file that must be loaded from IGVs server, is currently loaded

