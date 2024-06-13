# Welcome to IGV
This document is going to go serve as an introduction to IGV, including how to set it up, the major features of the program, and how to idenify differrent variants, including single nucleotide variants (SNVs), small variants from 2-50 bases (indels), and larger structural variants of at least 50 bases (SVs). There will also be examples and questions along the way to help this information sink in. 

For this tutorials a few peices of setup are required. First, the desktop apllications for nIGV can be downloaded [here](https://igv.org/doc/desktop/#DownloadPage/). You will have to select the right version of the application for your computer (Mac, PC, etc). Second, we will be using various files throughout this tutorial. These files can be found at `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial`. These files are
- `/storage1/fs1/jin810/Active/Projects/SMAHT/data/bam_sorted/hg002_sorted.bam`: This is a BAM file for the sample HG002, which we will use for examples at the end of this tutorial
- - `/storage1/fs1/jin810/Active/Projects/SMAHT/data/bam_sorted/hg005_sorted.bam`: This is a BAM file for the sample HG005, which we will use as examples thought this tutorial
- `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial/HG005_SNVs_CHR1.vcf` and `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial/HG005_SNVs_CHR1.vcf.idx`: The `.vcf` file contains a list of SNVs present in the first chromosme of HG005. the `.idx` file is an index file for the `.vcf` file. In IGV if the `.vcf` file is too large it uses an index file top help loads the data
- list of SVs in the proband
  
After conferming you have access to these files and have downloaded IGV continue on to the rest of the tutorial

# What is IGV
IGV, also know as the Integrative Genomics Viewer, is a tool developed by the broad. The goal of IGV is to allow users to manually viulaize different regions of a given patients genome. The power of this comes from if you know what reference contig a genome is aligned to and what a varisnt would look like in IGV, you can conferm weather a variant called by some variant caller is a true positive for a false positive. This cna greatly increase the sensetiviy and specificty of a given pipeline, especially those which are known to have high false positive rates. Keep in mind though IGV require manual labour, making iot difficult to visulize a large number of variants. The amount of variants possible to be visuzlied in a given time frame depends on the variants being looked at.

<details><summary>IGV Citation</summary>
&emsp; &ensp; Robinson JT, Thorvaldsdóttir H, Winckler W, Guttman M, Lander ES, Getz G, Mesirov JP. Integrative genomics viewer. Nat Biotechnol. 2011 Jan;29(1):24-6. doi: 10.1038/nbt.1754. PMID: 21221095; PMCID: PMC3346182.
</details>

# Setting up IGV
After opening IGV you should be greeted to a screen that looks similar to this

![Screenshot of empty igv window](/Images/Empty_IGV_Image.png)

There are a few key spots to point out
- Reference Contig: This is the reference contig what will be shown. It is important to make sure this matches the contig the bam files you intend to visualzie was aligned to
- current chromosme: this will tell you the curent cromosme your window shows. Keep in mind depending on the config it could have many more options that the standard 24 chomosome
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
Before loading any files you need to make sure you computer have access to files located on the RIS. To do so right click opn the finder application and select `Connect to server` Where is says `Server Address` type in the address of the server that has the files. for Jin810 the address is `smb://storage1.ris.wustl.edu/jin810` If the files you are intreset in is in another server make sure to find out the address for the server before trying to load any files

To load a file go to `File->Load from File` to load a file into IGV. There are two main types of files you will want to load
- Genomic sequencing files: These will be files with the actual sequecning of a pateints geneome. In almost all cases you will be loading a `.bam` or a `.cram` file. Of note for IGV to be able to load the file you must have the coresponding index file (either a `.bai` or `.crai`) in the same folder
- Variant list file: This file will have the list of vairants you want to visualzie, likely either a list of SNVs, Indels, or SVs. This could either be a `.vcf` or a `.bed` file.
If there is any other file you have stored locally or on the RIS that you think will help with your analysis (for example I have loaded encode data to see if any strucutal variants overlap with a known encoder), you can load it using this same method

There are a few addition files IGV provides that you may want to load for you analyis, for example list of know repeditive regions or list of known genes. To load these files go to `File->Load from Server`, Check the boxes for the files you want to load and click `OK` to load them.

Sessions repesent the list of files loaded into IGV at any given moment, along with the cuirrent window of the genome veing viewed. If you ever want to share an IGV window with someone or know you will be coming back to IGV to continue working on your current VCF, its useful to know how to save and load sessions
- to save a session to go to `File->Save Session`. This will create a `.xml` file for the session
- to load session go to `File->Load Session`. You can then select a `.xml` file and it will load that session into IGV.

### CHALANGE 1
Can you make your IGV look the same as the following screenshot

![Screenshot of empty igv window](/Images/Challange_One.png)

<details><summary>HINT 1</summary>
&emsp; &ensp; Make sure you have the right contig and window for your IGV session
</details>

<details><summary>HINT 2</summary>
&emsp; &ensp; Looking on the left side of the tracks will give you an idea of what file are currently loaded
</details>

<details><summary>HINT 3</summary>
&emsp; &ensp; Repeat masker, A file containing repetive region of the genone, must be loaded from IGVs server and is loaded in the screenshot
</details>

# IGV Controls
This section will go over some tips and tricks for using IGV
## Navigating IGV
It is important to know how to get around IGV and change what region of the genome you are looking at. Depending in what files you have loaded and how far away the new section you want to look at is there are various ways to view a new region
  - viewing a completly new region: If you have the cordinates of a new region the easier way to get there is by inputing those cordintae into the genomic region bar and clicking Go. Remeber to format the region as `chr#:start-stop` (for example `chr1:650,894-650,942`
  - if you just want to get to a new chomosome but don't know where in the chomosome, change the current chomosome to that and it will show yout the etirety of that chromome
  - if you want to zoom out of the currenty region you are looking at, click the - button on the top right of the screen to zoom out, the most you can zoom out to is an entire chomosome
  - likewise if you want to zoom in clikc the + button to the right of the - button. The most you can zoom into is 41 bases
  - another way to zoom in is by manually selecting the region you want to zoom in on. To do click on a base pair above the black devided slide you mouse to highlight a region. When you unclick you mouse you will zoom into the highlighted region
  - to maintain the same level of zoom but change the window to see a little bit before out after the current region click below the black divider and slide your mouse to the left to see the region right of the curent region and vise verse of thee the region left of the current region
  - Lastly, if you have a `.vcf` or `.bed` file loaded, you can go to the next or previous variant in the file by clickin `ctrl-f` and `ctrl-b` respectivly. This will be the best method for quickly visualzing a lot of variants
## Regions of interest
Of feature of IGV it you can select your current window as a region of intrest (ROI), which will highlight it in red and make it easier to come back to.

There are two ways to select your current region as a ROI.
1. go to `regions->region navigator` A secreen will pop up listing all the current regions of intrest. Clicking add will add your current window to the list
2. clicking `ctrl-r` will also add your current window to the ROI list.

There are also two ways to remove a region from the ROI list
1. if the ROI is in your curent window, you can clikc on it and select Delete to remove the region.
2. in addition if you go to `regions->region navigator` you can select multip[le ROI and selecting Remove will delete them all
## controling the look and sorting of the reads
Depending on what you are looking for you may want IGV to show you different peices of information about the reads to display the information in different ways. Here are a few of the key ways to help you visualize the data
1. grouping reads: Right clicking on a bam file

