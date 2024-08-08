Table of Content
- [Welcome to IGV](#Welcome-to-IGV)
- [What is IGV](#What-is-IGV)
- [Setting up IGV](#Setting-up-IGV)
  - [Loading Files and Sessions](#loading-files-and-sessions)
- [IGV Controls](#IGV-Controls)
  - [Navigating IGV](#Navigating-IGV)
  - [Regions of Interest](#Regions-of-interest)
  - [Controling the Look and Sorting of the Reads](#controling-the-look-and-sorting-of-the-reads)
  - [BLAT](#BLAT)
- [Identifying single nueclotide variants in IGV](#Identifying-single-nueclotide-variants-in-IGV)
- [Identifying small indels in IGV ](#Identifying-small-indels-in-IGV)
- [Identifying large sturcural variants in IGV](#Identifying-large-sturcural-variants-in-IGV)

# Welcome to IGV
This document is going to serve as an introduction to IGV, including how to set it up, the major features of the program, and how to identify different variants, including single nucleotide variants (SNVs), small variants from 2-50 bases (indels), and larger structural variants of at least 50 bases (SVs). There will also be examples and questions along the way to help this information sink in.

For this tutorial, a few pieces of setup are required. First, the desktop applications for IGV can be downloaded [here](https://igv.org/doc/desktop/#DownloadPage/). You will have to select the right version of the application for your computer (Mac, PC, etc). Second, we will be using various files throughout this tutorial. These files can be found at `/storage1/fs1/jin810/Active/testing/Ruttenberg/IGV_Tutorial`.
These files are
(Fill in at end)
  
After confirming you have access to these files and have downloaded IGV, continue on to the rest of the tutorial.

# What is IGV
IGV, also known as the Integrative Genomics Viewer, is a tool developed by the Broad Institute. The goal of IGV is to allow users to manually visualize different regions of a given patient's genome. The power of this comes from knowing what reference contig a genome is aligned to and what a variant would look like in IGV. You can confirm whether a variant called by some variant caller is a true positive or a false positive. This can greatly increase the sensitivity and specificity of a given pipeline, especially those known to have high false positive rates. Keep in mind, though, IGV requires manual labor, making it difficult to visualize a large number of variants. The number of variants possible to be visualized in a given time frame depends on the variants being looked at.

<details><summary>IGV Citation</summary>
&emsp; &ensp; Robinson JT, Thorvaldsdóttir H, Winckler W, Guttman M, Lander ES, Getz G, Mesirov JP. Integrative genomics viewer. Nat Biotechnol. 2011 Jan;29(1):24-6. doi: 10.1038/nbt.1754. PMID: 21221095; PMCID: PMC3346182.
</details>

# Setting up IGV
After opening IGV, you should be greeted with a screen that looks similar to this.

![Screenshot of empty igv window](/Images/Empty_IGV_Image.png)

There are a few key spots to point out:
- Reference Contig: This is the reference contig that will be shown. It is important to make sure this matches the contig to which the BAM files you intend to visualize were aligned.
- Current Chromosome: This will tell you the current chromosome your window shows. Keep in mind that, depending on the configuration, it could have many more options than the standard 24 chromosomes.
- Current Region: This shows you the exact coordinates of the window you are looking at in the form chr#:start-end. In addition, if there is a specific region you want to look at, you can input the coordinates in the same format and click 'Go'. You can also search for a specific gene, and it will show you the coordinates of that gene.
- Zoom In and Zoom Out: This will decrease or increase the size of the window respectively. Fairly self-explanatory.

There are a few settings you may want to adjust before visualizing any variants. To adjust the settings, go to `View -> Preferences`. The majority of the settings you will want to adjust are under Alignments:

- `Color alignments by`: This controls the color of each read, which doesn't really matter unless you are visualizing SVs (that is going to be a common trend), but it is recommended to have it set to INSERT_SIZE.
- `Group alignments by`: This will group reads of the same attribute together. Again, it only really matters for SVs, in which case you should have it set to PAIR_ORIENTATION.
- `Mapping quality threshold`: The mapping quality of a read is −10*log<sub>10</sub>Pr(mapping position is wrong). This threshold will remove all reads with a mapping quality below it. Thus, the number is larger the more accurate it is. We recommend setting the threshold to 1 to remove all reads where there is no confidence in where it is mapped to (you should not base any analysis on these reads).
- `Alignment display mode`: Controls how the reads look. Setting it to expand makes the reads as large as possible, which makes them easier to interpret.
- `Show soft mismatched bases and Show soft-clipped bases`: Controls if you see bases that don't match the reference. Both should be checked as we are looking for deviations from the reference sequence.

With these settings, you are ready to begin visualizing BAM files.

## Loading files and sessions
Before loading any files, you need to make sure your computer has access to files located on the RIS. To do so, right-click on the Finder application and select `Connect to Server`. Where it says Server Address, type in the address of the server that has the files. For Jin810, the address is `smb://storage1.ris.wustl.edu/jin810`. If the files you are interested in are on another server, make sure to find out the address for that server before trying to load any files.

To load a file, go to `File -> Load` from File to load a file into IGV. There are two main types of files you will want to load:
- Genomic sequencing files: These will be files with the actual sequencing of a patient’s genome. In almost all cases, you will be loading a `.bam` or a `.cram` file. Of note, for IGV to be able to load the file, you must have the corresponding index file (either a `.bai` or `.crai`) in the same folder.
- Variant list file: This file will have the list of variants you want to visualize, likely either a list of SNVs, Indels, or SVs. This could be either a `.vcf` or a `.bed` file.
If there are any other files you have stored locally or on the RIS that you think will help with your analysis (for example, I have loaded ENCODE data to see if any structural variants overlap with a known ENCODE element), you can load them using this same method.

There are a few additional files IGV provides that you may want to load for your analysis, such as a list of known repetitive regions or a list of known genes. To load these files, go to `File -> Load from Server`, check the boxes for the files you want to load, and click `OK` to load them.

Sessions represent the list of files loaded into IGV at any given moment, along with the current window of the genome being viewed. If you ever want to share an IGV window with someone or know you will be coming back to IGV to continue working on your current VCF, it's useful to know how to save and load sessions:
- To save a session, go to `File -> Save Session`. This will create a `.xml` file for the session.
- To load a session, go to `File -> Load Session`. You can then select a `.xml` file, and it will load that session into IGV.

### CHALANGE 1
Can you make your IGV look the same as the following screenshot?

![Screenshot of empty igv window](/Images/Challange_One.png)

<details><summary>HINT 1</summary>
&emsp; &ensp;Make sure you have the right contig and window for your IGV session.
</details>

<details><summary>HINT 2</summary>
&emsp; &ensp;Looking on the left side of the tracks will give you an idea of what files are currently loaded.
</details>

<details><summary>HINT 3</summary>
&emsp; &ensp; RepeatMasker, a file containing repetitive regions of the genome, must be loaded from IGV's server and is loaded in the screenshot.
</details>

# IGV Controls
This section will go over some tips and tricks for using IGV.

## Navigating IGV
It is important to know how to navigate IGV and change the region of the genome you are viewing. Depending on the files you have loaded and how far away the new section you want to view is, there are various ways to view a new region:

- Viewing a completely new region: If you have the coordinates of a new region, the easiest way to get there is by inputting those coordinates into the genomic region bar and clicking `Go`. Remember to format the region as `chr#:start-end` (for example, `chr1:650,894-650,942`).
- Navigating to a new chromosome: If you just want to get to a new chromosome but don't know where in the chromosome, change the current chromosome to that one, and it will show you the entirety of that chromosome.
- Zooming out: If you want to zoom out of the current region you are viewing, click the `-"=` button on the top right of the screen to zoom out. The most you can zoom out to is an entire chromosome, though you won't be able to see individual reads when zoomed out more than 20kb.
- Zooming in: To zoom in, click the `+` button to the right of the `-` button. The most you can zoom in to is 41 bases.
- Manual zooming: Another way to zoom in is by manually selecting the region you want to zoom in on. To do this, click on a base pair above the black divider, slide your mouse to highlight a region, and release the mouse button to zoom into the highlighted region.
- Adjusting the window: To maintain the same level of zoom but change the window to see a bit before or after the current region, click below the black divider and slide your mouse to the left to see the region right of the current region, and vice versa to see the region left of the current region.
- Navigating variants: Lastly, if you have a `.vcf` or `.bed` file loaded, you can go to the next or previous variant in the file by pressing `ctrl-f` and `ctrl-b` respectively. This is the best method for quickly visualizing a lot of variants.

## Regions of interest
One feature of IGV is that you can select your current window as a region of interest (ROI), which will highlight it in red and make it easier to return to.

There are two ways to select your current region as a ROI:

1. Go to `Regions->Region Navigator`. A screen will pop up listing all the current regions of interest. Clicking `Add` will add your current window to the list.
2. Pressing `ctrl-r` will also add your current window to the ROI list.

There are also two ways to remove a region from the ROI list:

1. If the ROI is in your current window, you can click on it and select `Delete` to remove the region.
2. Additionally, if you go to `Regions -> Region Navigator`, you can select multiple ROIs and selecting `Remove` will delete them all.

## Controling the look and sorting of the reads
Depending on what you are looking for, you may want IGV to show you different pieces of information about the reads and display the information in various ways. Here are a few key ways to help you visualize the data:

### Grouping reads
Right-clicking on a BAM file will show the option `Group Alignments By`. Hovering your mouse over it will reveal different options to group the alignments.

A few of the most useful options are:

- Read Strand: This separates reads sequenced in the forward direction from those sequenced in the reverse direction.
- Chromosome of Mate: This separates reads by the chromosome to which their paired reads are aligned. This can be useful in identifying large insertions or translocations.
- Pair Orientations: This separates the reads by pair orientation (RL, LR, RR, or LL). This is useful in identifying inversions and duplications, as well as some insertions. For more information on pair-end orientation, see [here](https://gatk.broadinstitute.org/hc/en-us/articles/360035531792-Paired-end-or-mate-pair).

### Sorting reads 

Right-clicking on a BAM file will show the option `Sort Alignments By` directly below `Group Alignments By`. This will sort the reads by the given criteria. Note that the sorting is based on your current window, so if you move to a new region of the genome, you will have to re-sort the reads to your desired arrangement.

A few of the most useful options are:

- Start Location: This sorts each read by the start location of the read (always the lowest base). This is useful for grouping reads with similar features together, such as grouping SNVs or SRs.
- Mapping Quality: This sorts the reads by mapping quality, bringing all the most confidently mapped reads to the top.
- Insert Size: This sorts by insert size. This will group all the reads with larger or smaller than expected insert sizes together, helping to show if there is insert size evidence for structural variants.
- Reverse Sorting: This is not a sorting strategy on its own, but if selected, it will reverse the sorting strategy you choose (such as start location or insert size), sending the reads originally at the bottom to the top and vice versa.


### Coloring reads
Like grouping and sorting reads, `Color Reads By` is an option available by right-clicking the BAM file and provides various options for coloring the reads.

Some of the most commonly used options are:

- Insert Size: Read pairs with a larger or smaller than expected distance between the two reads will be colored red and blue, respectively. This is useful in identifying various structural variants.
  - To know the exact insert size of two reads, click on the read and look for the value next to Insert Size.
    - If the value is negative, that means you clicked on the second of the two reads. The absolute value will be the insert size.
- Pair-End Orientation: Colors forward-reverse reads (FR) grey (no color), RF green, FF cyan, and RR blue. This will also color each read where the pair is mapped to a different chromosome by various colors depending on the chromosome.
  - Note that there is an option to color by both insert size and pair-end orientation. This works well for seeing both pieces of information but suffers from the issue that if a read has an abnormal insert size and not a FR pair-end orientation, it will be colored by the pair-end orientation, not the insert size. For this reason, I recommend coloring by only insert size and grouping by pair-end orientation. This allows you to see both pieces of information at the same time.

For grouping alignments, sorting alignments, and coloring alignments, there are many more options, so feel free to explore the choices to see if there are additional options that will work with how you are trying to use IGV.

### Showing bases
There are a few different controls users have over what bases are shown and how they look. You can change these settings by right-clicking the BAM file:

- Show All Bases: If this is checked, instead of seeing grey lines for your reads, you will see the exact sequence at every base for every read. I personally find this to be too much information at once and rarely use it, but it can be helpful if you need to know the exact sequence of a specific region that is larger than the mismatched bases.
- Show Mismatched Bases: Selecting this option will show a grey bar for any base in a read that matches the reference sequence and will show the exact bases if it does not match the reference. I find this setting the most useful, as we are often only interested in variations from the reference.
  - Note: Unless you select `Show Soft-Clipped Bases` as described earlier, this will only show you when an individual base differs from the reference. You must have this option selected and have it set up to show soft clips in order to see the soft clips.
- Shade Base by Quality: This will shade any shown bases (not the grey bar) based on how confident the sequencing of that base is. The closer the base looks to grey, the less confident the sequencing is. To see the exact quality of the sequencing of the base, click on the base and look for the number next to `QC`.
### View as pairs
Another useful setting you can turn on and off by right clicking the bam file is `View as pairs`. Turning this on will put the two readsn in the read pair on the same row and show a grey line connecting them. This is useful as it lets to manually see the instert size and if it is larger or smaller than expected. If you want to sort by insert size to see if there is any variation it is highly recommended you turn on this setting first. One downside is it makes it harder to group reeads overlapping the same sequence together, making it harder to see SNVs or soft clips

### Challange.
Can you make your IGV look like this?
Show some image

## BLAT
Mutations are commonly a result of genomic rearrangements, meaning mutations come from other locations in the genome. Thus, it can be extremely useful to find the most likely location for a genomic sequence from the reference sequence. This is useful because the location a read maps to is not always the sequence of the reference genome it matches the best. The best way to map a sequence to the reference is using BLAT. Thankfully, BLAT is built into IGV, making it easy to use. This section will go over the easiest ways to BLAT a sequence.

### Using BLAT on a user defined sequence.
The most basic way to use BLAT is to input a sequence, and BLAT will tell you where in the reference sequence best matches that sequence. To use BLAT this way, go to `Tools -> BLAT`. You should be prompted to `Enter a sequence to BLAT`. Type in your sequence and press `OK` to run BLAT on that sequence.

- Note 1: BLAT always requires a reference sequence to check the input sequence against. In IGV, this sequence will always be the reference assembly selected. To change the reference used for BLAT, you must change the reference assembly in the top-left corner.
- Note 2: Due to the low complexity of DNA, which only has 4 base options, the sequence you input must be at least 20 bases long.
### Challenge
where in refernce assembly hg38 does the following sequence best match. `TATTTTAGTACATATATGTAATTCACAAATAAATGAATATATAAATATTTGTAGAGTGTGCCTAAAATTTATAACTGATAGTACATGTAATCAGTCAAGGTTAAGGACTCTGCTATCTTCCCGCGACTGGGCCCAAATCCCTAATGGAGAG`
<details><summary>Answer</summary>
&emsp; &ensp; The sequence best matches to chr1:159692997-159693148 and hasd a score of 986
</details>

### Finding the most likely location for a read
Another useful thing you can do in IGV is run a BLAT search on the sequence of a read. This can be useful because reads don't always align to the place they match most, instead aligning differently based on the alignment of their pair. It is also useful because there are many repeat regions or similar sequences in the genome, and knowing a read has other similar matches can be useful in variant detection. Finally, if the read spans a deletion compared to the reference sequence, BLATting the read will show you the deletion. To run a BLAT search on a read, right-click the read and select `BLAT Read Sequence`.

### Challenge 
Set your window to `chr1:105963222-105963498`. You should see multiple red reads in the RL track (see below). Blat on of these red reads. About how many different aligments havew a score greater than 600
<details><summary>Answer</summary>
&emsp; &ensp; Depending on the read you selected, there should be between 5 and 10 locations with a score greater than 600
</details>

### Searching Soft clips with blat
Lastly, and arguably more usefully, you can use BLAT to search a soft clip. To do so, right-click on the read with the soft clip and select `BLAT Left-Clipped Sequence` or `BLAT Right-Clipped Sequence` depending on which side of the read has the soft clip. Keep in mind that the soft clip must be at least 20 bases long to be used as input for BLAT, so the option will not show up if there is not a soft clip long enough.

### challenge
Go to the window `chr6:68,014,727-68,015,341`. On the left side of the screen should be right soft-clips. On the right should be left soft clips. Right click on the reads with left cloft clips. Where does it best align to
<details><summary>Answer</summary>
  &emsp; &ensp; Depending of the read you selected the end position could be different, but the start position should be chr6:68015268. Follow up question. Note how close this is the the read itself. What could that mean. If you are unsure come back to thsi question afetr finishing the guide and you should be able to answer the question.
</details>

With this, you should have enough information for most tasks you would want to accomplish in IGV. Keep in mind there are many other tools not discussed in this guide. It is highly recommended to experiment with other tools to see what else you can do. For more information on what you can accomplish using IGV, refer to [this](https://igv.org/doc/desktop/#UserGuide) guide

# Identifying single nueclotide variants in IGV 
As with all variant confirmation in IGV, you will need a VCF of candidate variants to either confirm as true positives or label as false positives. For this example, the candidate SNVs can be found at `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_snvs_chr1.vcf`.
In addition, make sure you have Show mismatched bases on to be able to see the variants.

Identification of SNVs is extremely straightforward as IGV will label when there is an SNV or not. The basis of SNV identification is to go to the location of a candidate SNV and see how many of the reads at that position show the alternative allele. For a heterozygous mutation, it should be about 50% of the reads. For a homozygous case, it should be 100% of the reads. These two cases can be seen below.

![Screenshot of empty igv window](/Images/Heterozygous_SNV.png)

![Screenshot of empty igv window](/Images/Homozygous_SNV.png)

The top screenshot shows the case of a heterozygous mutation, where the alt allele A is in about 50% of the reads, and the reference allele G is in the other 50%. The lower screenshot shows a homozygous mutation where the alt allele C is in all the reads and the reference G is not seen at all.

To get the exact percentage of reads with the alternative allele, click the base in the read depth track (in this case, it is the green/orange rectangle above the A's and the blue rectangle above the C's, respectively).

In addition, you may see a case like the following.

![Screenshot of empty igv window](/Images/Somatic_SNV.png)

While the signal is a bit messy here (which is common with real data), we can see that the majority of the reads match the reference. However, there is a significant number of reads that show a C at this base instead of a T. There could be a few explanations for how this occurred. Firstly, this could be a heterozygous mutation; however, the probability of only 14 or fewer of the 68 reads being the alternate allele is 0.00000042, which is highly unlikely. It could be a sequencing error as well, but SRS has a relatively low error rate of about 1% (depending on the sequencing tool), so the odds of the same sequencing error occurring 14 times in 68 reads is again extremely low. Thus, the most likely explanation is that it is a somatic mutation, or a mutation that is not in all cells. There is no hard cutoff percentage for somatic vs. heterozygous vs. homozygous, so you will need to decide beforehand how you will classify them.

### challenge
How would you label the following mutations in `WGS_001_Father`? All the following mutations can be found in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_snvs_chr1.vcf`
1. chr1:72008005
2. chr1:72011871
3. chr1:72022965
4. chr1:72105973
5. chr1:72113026
6. chr1:72114987
7. chr1:72120659
   
<details><summary>Answers</summary>
&emsp; &ensp;

1. Homozygous
2. Heterozygous
3. Somatic (37% of the reads showing the alt allele in a heterozygous mutation is unlikely but very possible. Depending on your criteria, a classification of heterozygous or somatic could be appropriate.)
4. Heterozygous
5. Heteroygous and somatic (the T->C Mutation appears to be heterozygous and the T->G mutatation looks somatic being in 3% of reads)
6. Homozygous
7. mutliallelic heterozygous(one chromosome is A->C, the other is A->T)
</details>

# Identifying small indels in IGV 
Indels can be broken down into insertions and deletions. Most of the time, indels are clearly visible in IGV. However, there may be some edge cases that are more difficult to see. We will go over all these cases in the section below. Additionally, there are a few settings that must be enabled to detect the indels. Like with SNVs, `Show mismatched bases` must be on. Also, make sure `Label indels > threshold` is checked and `Label threshold (bases)` is set to 0. Finally, ensure that `Show soft-clipped bases` is checked. For the last two settings, they can be found in `View -> Preferences -> Alignments`.

## insetions
Insertions occur when new bases are inserted between two bases in the reference sequence. These can be seen in IGV by looking for purple bars with white numbers on them in the reads (see below). The location in the read will always be between two bases, indicating where the sequence is inserted into the DNA, and the number represents how many bases are inserted. To see the exact sequence inserted, click on the purple bar.

![Screenshot of empty igv window](/Images/indel_insertions.png)

## Deletions
Deletions occur when a small sequence of DNA is removed compared to the reference sequence. Like with insertions, IGV will directly show these on the reads. In this case, a part of the read will be replaced with a black line and a purple number. That number indicates the number of bases missing. To get the sequence missing, you can look at the sequence track and see what bases the gap overlaps.

![Screenshot of empty igv window](/Images/indel_deletion.png)

Like with SNVs, indels can be categorized as heterozygous, homozygous, or somatic based on the allele frequency (AF). However, unlike SNVs, there is no easy way to get the exact number of reads showing the mutation without manually counting them. For deletions, you could compare the read depth outside the mutation versus inside the mutation, but these values are subject to noise and misalignment, making them less reliable. Additionally, keep in mind that indels in multiple locations could be the same indel if the region is a repeat.

![Screenshot of empty igv window](/Images/indel_repeat_regeion.png)

In this screenshot, IGV shows a deletion of the sequence `TTTTTC` from `chr1:155568828-155568833`. However, this region is a repeat of the `TTTTTC` sequence, so it is impossible to know which repeat is missing. Thus, if the deletion was labeled from `chr1:155568834-155568839`, it would be referring to the same indel.

Finally, for larger indels (by definition, indels cannot be larger than 50 bases; otherwise, they would be categorized as structural variants), it is possible for a read to not span the entire mutation. This prevents IGV from showing the SV. Instead, you will see soft clips at the end of the read.

![Screenshot of empty igv window](/Images/indel_del_with_soft_clips.png)
![Screenshot of empty igv window](/Images/indel_ins_with_soft_clips.png)

In these two examples, we can see that most of the reads label the indel. However, some of the reads just show a sequence of mismatched bases at the end. This is a soft clip. What IGV is indicating is that up to a certain point, this read is aligning with the reference sequence, but at the soft clip, it is no longer a match.

For the deletion, the soft clip sequence (the bases in the read) corresponds with the sequence after the labeled indel. This matches with the other reads showing the indel, as all the reads show the consensus sequence with those 6 bases missing. IGV is unable to label the indel on these reads because it does not have enough of the sequence following the soft clip to be sure that the sequence is from that region.

In the case of insertions, we can see that it is an insertion of the bases CA. However, IGV shows more than those bases in the soft clip. Looking at the soft clip sequence, we see the CA following the expected reference sequence before the indel. This matches with the other indels, as it shows the reference sequence before the indel, followed by the indel sequence, and then the reference sequence after the indel (the sequence after is not shown since it is aligning with the reference). Again, it is shown as a soft clip rather than an indel since IGV does not have enough of the sequence before the indel to be sure that is where the sequence is from. Keep this in mind if you are visualizing indels and see soft clips rather than indel markers.

### Challenge
How would you label the following mutations in `WGS_001_Father`? All the following mutations can be found in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_indels_chr1.vcf`

1. chr1:28932407
2. chr1:28947408
3. chr1:28990296
4. chr1:48460717
5. chr1:48497209
   
<details><summary>Answers</summary>
&emsp; &ensp;

1. Homozygous insertion 
2. Heterozygous deletion
3. Somatic insertion
4. Heterozygous deletion
5. Heterzygous Interstion
</details>

# Identifying large sturcural variants in IGV 
Structural variants are variants impacting more than 50 bases. They are the most complicated to detect in IGV because most sequencing is short read and does not span the entire mutation. Therefore, IGV is unable to directly show you the mutation, and you must use clues to infer the presence of an SV. This guide will cover the major SVs you may encounter and how they will appear in IGV. For more details, including how to visualize complex structural variants, refer to the following slides: ([Visualize common SVs](https://docs.google.com/presentation/d/13rQ7rrYeOpSAon3BPFgQPzYyYhBAPeLo_uVq0ET3s7E/edit?usp=sharing), [Visualize complex SVs](https://docs.google.com/presentation/d/1dCcq4Rq3-AJASmq7eK62l8YLsMMCMNWzQjSJYnHTcgo/edit?usp=sharing))

## Overview of evidence
All the pieces of evidence you will want to use have been mentioned before in this guide, but here we will consolidate them into one place. For a visual example of each piece of evidence, refer to the image below.

![Screenshot of empty igv window](/Images/IGV_Evidence.png)

### Read Depth
Read depth is the number of reads that cover an individual base. Any read that aligns over a base will increase the read depth unless it is a soft clip at that location. The read depth is shown in IGV as the grey bar at the top of the BAM file track. For variants that either duplicate or delete regions of the genome, there will be a change in the read depth that reflects the genomic content being added or removed. However, read depth may not always show a sharp change, as seen in the image above. In a repeat or duplicated region, a read spanning a deleted or duplicated region may look very similar to if the variant were not there. This tends to result in a gradual increase or decrease in the read depth.
### Soft Clips
Soft clips, also known as split reads, are parts of an individual read that contain genomic content from a different location. This can occur when a read spans the breakpoint or boundary of a variant. This makes soft clips very useful in identifying the exact coordinates of a variant. However, similar to read depth, if the variant is in a duplicate or repeat region, there may not be a soft clip. Since it is valuable to know where the soft clip sequence is from, BLAT is a very powerful tool for understanding soft clips.
### Insert Size
Insert size refers to the distance between the two paired reads in short read sequencing. For most short read sequencing (SRS), this value tends to be around 300 bases, give or take 100 bases. However, if there is a structural variant in the region of the read, it can result in the distance between the two reads, when aligned to the reference, being much larger or smaller than expected. If a read is labeled in blue, it means this distance is smaller than expected, and if it is red, it means it is larger than expected. Additionally, if the two reads align to different chromosomes, instead of being colored red or blue, they will be colored based on the chromosome of the pair.
### Pair-end Orientation
In short read sequencing, the first read is sequenced from 5' to 3', and the second read is sequenced from 3' to 5'. A read in the 5' to 3' direction is referred to as being in the left direction (don’t ask why), and the 3' to 5' read is in the right direction, meaning the pair together is LR. This is the paired-end orientation. However, large variants can change this orientation, resulting in RL, LL, and RR reads. These are labeled as green, cyan, and blue, respectively.

With these four pieces of evidence established, we will now go into detail on the main five kinds of variants: deletions, duplications, inversions, insertions, and translocations. It is important not just to understand the evidence but also to understand why the evidence looks this way, so make sure to spend time understanding how the signal occurs.

## Deletion
A deletion is the removal of a large segment of the genome. They tend to be easier to detect because the evidence for identifying a deletion is straightforward. You want to look for three main pieces of evidence:
1. Read Depth: There will be a drop in the read depth. This drop would likely be about 50% of the read depth around for a heterozygous mutation and 100% of the read depth for a homozygous mutation.
2. Insert Size: If the two reads span the deletion, there is more genomic information between the reads when aligning to the reference, resulting in an insert size larger than expected. This would be labeled in red.
3. Soft Clips: The soft clips should be inward and show the sequence on either side of the deletion.

Understanding these pieces of evidence will help in accurately identifying deletions in your data.
![Screenshot of empty igv window](/Images/SV_DEL.png)

In the image above, note that the BLAT alignment at the bottom shows that the soft clip sequence by the left soft clip corresponds with the reference sequence by the right soft clip, and vice versa. Additionally, performing a BLAT alignment of the entire read with the soft clip highlights the gap. This evidence, combined with the red insert size reads and the complete loss in read depth, confirms that this is a homozygous deletion.

## Duplications
Unsurprisingly, a duplicated region of the genome in IGV looks like the opposite of a deletion. You should see:
1. Increased Read Depth: There will be more reads from the duplicated region, resulting in an increase in read depth.
2. Blue Insert Size Labels: This indicates that the reads are closer than expected because the duplicated regions between two reads are not in the reference, causing the two reads to be closer together.
3. Outward Soft Clips: These soft clips show the other breakpoint and correspond with reads spanning the boundary between the two duplicated regions.
4. Green Pair-End Orientations: Showing RL orientations. This occurs because if a read is from the end of the first duplication and its pair sequences the start of the second duplication, the second read will map before the first read.

All this evidence together could look like the following.
![Screenshot of empty igv window](/Images/IGV_Evidence.png)

It is important to note that most duplications, and in fact most variants, will not show all possible evidence. For instance, if the duplication is too small, there may not be pair-end orientation evidence. Conversely, if the duplication is too large, the reads might not span the duplication, so there may be no insert size evidence. It is crucial to consider the evidence present in IGV and determine which, if any, of the variant types best match the evidence observed.

## Inversion
An inversion occurs when a region of the genome flips directions, so the end of the region becomes the start and vice versa. An important consequence of this is that since the 3' end becomes the 5' end, but the strand remains 5' to 3', it will flip to the complementary strand. As there is no genomic information gained or lost, there should not be a change in read depth. 

There could be
1. insert size evidence: based on how large the inversion is, the reads could be labeled blue or red. The most important evidence for identifying an inversion is
2. soft clips: Soft clips should be present on both sides of the breakpoints. However, since inversions often occur due to crossover between similar sequences in the genome, soft clips may not always be present.
3. pair-end orientations: Mosgt inportantly you will see RR and LL pair-end orientations. 

![Screenshot of empty igv window](/Images/SV_INV.png)

In this case, there are no breakpoints outward at the first breakpoint, and there is also a change in the read depth in this region. Abnormalities like these are common in inversions. However, the LL and RR reads, along with the soft clip sequences being found in the nearby region, are sufficient to confirm that this is an inversion.


## Translocation
A translocation is a swap between genomic content on two chromosomes. They are the only variant where, in IGV, you will see only one breakpoint. At this breakpoint, you should see soft clips on both sides. Additionally, there should be many reads whose mate is mapped to a different chromosome, and these mates should all be on the same chromosome. Since this impacts two chromosomes, you should also see similar signals on the other chromosome.

![Screenshot of empty igv window](/Images/SV_CTX_1.png)
![Screenshot of empty igv window](/Images/SV_CTX_2.png)

Note we need two images. One from chromosome 12, one from chomosome 13. In each of these we see the same color for the reads, the ones in chr12 are the color for chr13 and vise versa. This is the defining singal for a translocation. Also note this is not a screenshot from WGS001_Father like everything prior. This is because translocations are extremmly rare. This is the only sample I have access to (out ouf 100+) that I knew had a translocation. Keep this in mind if you thing you see a translocation or are trying to conferm one.

## Insertion
We have saved insertions for last because they are the most complex to detect. Insertions are additional bases added to a chromosome, either from elsewhere in the genome or as random sequences. The complexity arises from the various mechanisms that cause insertions and how the signal can look very similar to other variants.

In general, the read depth will not change with an insertion. However, depending on the mechanism, there could also be a small deletion or duplication of an insert site, typically less than 50 bases. Thus, if you see a read depth change of less than 50 bases, consider the possibility of an insertion, but remember that the lack of a read depth change could also indicate an insertion.

The pair-end orientation and insert size can vary based on where the inserted sequence is from and how large it is. This evidence is generally harder to use for insertions. Finally, you will see soft clips on both sides of the breakpoint(s). Thus, insertions can look like any of the following.

![Screenshot of empty igv window](/Images/SV_INS_RANDOM.png)
![Screenshot of empty igv window](/Images/SV_INS_DEL.png)
![Screenshot of empty igv window](/Images/SV_CTX_2.png)

Note the sequence `GCCTGAAATGATCTTCCTCCGTCTTACCTATGAACTGGTCCTATCTTCCCTCTTCATTCACAAATATTCATTAGGTGC` that is present in both soft clips of the first screenshot. This is the inserted sequence. By examining the reads and removing this sequence, you will see that it matches the reference sequence with a duplication of the insert size (which is why we observe an increase in read depth). This is a classic signal for an insertion of a random sequence. It can be confirmed that the inserted sequence is random since attempting to BLAT the soft clip will yield no strong matches.

The second screenshot represents a similar case to the first; however, instead of an insert size duplication, a few bases have been deleted. This is likely due to non-homologous end joining but is another case of an insertion.

The third screenshot is a classic example of a mobile element insertion (MEI). MEIs always have insert site duplications and tend to have reads whose mates map to different chromosomes, but not a consistent chromosome. This occurs because they are mapping to different instances of the same transposable element throughout the genome.

![Screenshot of empty igv window](/Images/SV_INS_1_12_image_1.png)
![Screenshot of empty igv window](/Images/SV_INS_1_12_image_2.png)

These last two images show the same insertion. Note the consistent color of the reads indicating that their mates map to different chromosomes. In the first image, these reads map to chromosome 12. When you examine this region in chromosome 12, you see the second image, where the reads map to chromosome 1.

In the second image, although it might not initially look like an insertion because the breakpoints are far apart, this is an insertion rather than a translocation. The region between the two breakpoints in chromosome 12 has been inserted into chromosome 1.

## False Positives
Due to short read sequencing's difficulty with SV detection, it can have a high false positive rate. Therefore, it is important to understand how false positives can occur. False positives are often caused by repetitive regions of the genome. A consistent way to identify false positives is by examining the soft clips. If it is a true positive, the soft clips should be consistent across all the reads. However, in these two images, there is no consistent sequence in the soft clips. This inconsistency means these are false positives and should be discarded.

![Screenshot of empty igv window](/Images/FP1.png)
![Screenshot of empty igv window](/Images/FP1.png)


# QUIZ TIME!
It can be very difficult to understand how to visualize variants without practice, so go through the 50 cases in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.practice.vcf`. These all represent regions in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.bam`. Label them as true positives or false positives, and if they are true positives, specify the variant class (SNV, indel, or SV), their frequency in the genome (somatic, heterozygous, or homozygous), and for indels and SVs, the type of variant (insertion, deletion, duplication, inversion, or translocation).

Two notes about labeling:

- A somatic variant will be one in less than 30% of reads.
- An indel will be 50 bases or less, with a structural variant being 51 bases or more.
Keep in mind these definitions can change based on the project, so don't use these thresholds exactly going forward. This is just to provide consistency in how we label the variants.

After analyzing those 20 variants, go to `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.answerkey.vcf` to see the answer. Feel free to check in with Andrew if you have any discrepancies between your labeling and the answer key.


