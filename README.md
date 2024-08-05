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
like grouping and sorting reads, `color reads by` is an option by right clicking the bam file and have various options for how to color the reads. 

Some of the mosty commonly used options are
- insert size: read pairs with a larger than expected distance between the two reads and smaller than expected distance will be colored red and blue respecitvly. This is useful in idenifying various structual variants.
  - to know the exact insert size of two reads click on the read and look for the value next to `insert size`
    - if the value is negitive that means you clicked on the second of the two reads. The absolute value will be the insert size 
- pair end orientation: colors forward reverse reads (FR) grey (no color), RF Green, FF cyan, and RR Blue. This will also color each read where the pair is mapped to a different chromosome by various colors depending on the chomosome
  - Note there is an option ti color by insert size and pair end orientation. This works well for seeing both peices of information, but suffers from in a read has an abnosmal insert size and not a FR pair end orientation it will color the read by the pair end orientation, not the insert size. It is for that reason I recommended coloring by only insert size and grouping by pair end orientation, it allows you to see both peices of information at the same time   

  For group aligments, sort aligments, and color aligments there are many more options so feel free to play around with the choices to see if there are additions options that will work with how you are trying to use IGV

### Showing bases
There are a few different controls users have over what bases are shown and how they look. You can change these seting by right clicking the bam file
- `show all bases`: if this is checked than instread of seeing grey lines for you reads you will see the exact sequence at every base for every reads. I persoanlly find this to be too much information at once and rarly use it, but is can be helpful if you need to know the exact seqence of a specificly region that is larger than the mismatched bases
- `show mismatched bases`: having this option selected will show a grey bar for any base in a read that matches the reference sequence, and will show the exact bases if it does not match the reference. I find this setting the most useful as more often then not we are only intrested in variations from the reference.
   - Note: unless you selected `show soft-clipped bases` as descriped earlier, this will only show you when an indivual bases differs from the reference. You must have this option selected and have it set up to show sloft clips in order to be able to see the soft clips
- `shade base by quality`: This will shade any shown bases (not the grey bar) bases on how cofiudenct the sequencing of that base is. The closer the base looks to grey the less confidnet the sequencing is. To see the exact quality of the seuqenbciong of the base click on the base and look for the number next to `QV`

### View as pairs
Another useful setting you can turn on and off by right clicking the bam file is `View as pairs`. Turning this on will put the two readsn in the read pair on the same row and show a grey line connecting them. This is useful as it lets to manually see the instert size and if it is larger or smaller than expected. If you want to sort by insert size to see if there is any variation it is highly recommended you turn on this setting first. One downside is it makes it harder to group reeads overlapping the same sequence together, making it harder to see SNVs or soft clips

### challange.
Can you make your IGV look like this
Show some image

## BLAT
Mutations are commonly a result of genomic rearagments, meaning mutations come from other locations in the genome. Thus is can be exreemly useful the find the most likely location for a genomic sequnece from the reference sequence. This is useful as the location a read maps to is not always the seqence of the reference genome it matches the best. The best way to map a sequence to the reference is using BLAT. Thankfully BLAT is built into IGV, making it easy to use. This section will go over the easier ways to BLAT a seqence
### Using BLAT on a user defined sequence.
The most basic way to use BLAT is to input a sequence and BLAT will tell you where in the reference best matches that seqence. To use BLAT this way go to `Tools->BLAT`. You should now be prompted to "enter a sequence to blat" Type in your sequence and press "OK" to run blat on that seqence.
- Note 1: Blat always requires a reference sequence to check the inout sequence against. In IGV this sequence will always be the reference assembly selected. To change the refernece use for blat you  us chnage the refernce assembly in the top left corenr.
- Note 2: Due to the low complexity of DNA only have 4 base option, the sequence you input must be at least 20 bases
### Challenge
where in refernce assembly hg38 does the following sequence best match. `TATTTTAGTACATATATGTAATTCACAAATAAATGAATATATAAATATTTGTAGAGTGTGCCTAAAATTTATAACTGATAGTACATGTAATCAGTCAAGGTTAAGGACTCTGCTATCTTCCCGCGACTGGGCCCAAATCCCTAATGGAGAG`
<details><summary>Answer</summary>
&emsp; &ensp; The sequence best matches to chr1:159692997-159693148 and hasd a score of 986
</details>

### Finding the most likely location for a read
Another useful think you can do in IGV is run a BLAT search on the sequence of a read. This can be useful as reads don't away align to the place they match most, insteads aligning differenty based on the aligment of its pair. It is also useful as there are many reapst region or simular sequnces in the genome and knowing a read has other simlar matches can be usful in variatn detection. Finally if the read spans a deletion compared to the refernce sequence, blating the read will show you the deletion. To run a BLAT search on a read right click the read and select `BLAT read sequence` 
### Challenge 
Set your window to `chr1:105963222-105963498`. You should see multiple red reads in the RL track (see below). Blat on of these red reads. About how many different aligments havew a score greater than 600
<details><summary>Answer</summary>
&emsp; &ensp; Depending on the read you selected, there should be between 5 and 10 locations with a score greater than 600
</details>

### Searching Soft clips with blat
lastly, and arguably more usefully, you can use blat to search a soft clip. To do so again right click on the read with the soft clip, and select `BLAT left-clipped sequence` or `BLAT right-clipped sequence` depending on which side of the read has the soft clip. keep in mind that the soft clip must be at least 20 bases to be used as an input for blat, so the option will not show up if there is not a coft clip long enough

### challenge
Go to the window chr6:68,014,727-68,015,341. On the left side of the screen should be right soft-clips. On the right should be left soft clips. Right click on the reads with left cloft clips. Where does it best align to
<details><summary>Answer</summary>
  &emsp; &ensp; Depending of the read you selected the end position could be different, but the start position should be chr6:68015268. Follow up question. Note how close this is the the read itself. What could that mean. If you are unsure come back to thsi question afetr finishing the guide and you should be able to answer the question.
</details>

With this you should have enough information for most tasks you would want to accompish in IGV. Keep in mind there are many other tools not discussed in this guide. It is highly recommend to expirence with other tools to see what else you can do. For more information of what you can accomplish using IGV, refer to [this](https://igv.org/doc/desktop/#UserGuide) guide

# Identifying single nueclotide variants in IGV 
As will all variant confermation in IGV, you will need a vcf of candidate variants to either conferm as true positives or label as false positive. For this example the candidate SNVs can be found at `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_snvs_chr1.vcf`
In addition make sure you have `Show mismatched bases` on to be able to see the variants

idenfitication of SNVs is extreemly straight forward as IGV will label when there is an snv or not. The basis of SNV idneification is you go to the location of a canidiate SNV and see how many of the reads at that position show the alternative allele. For a heterozygous mutation is should be about 50% of the reads. For a homozygous case it should be 100% of the reads. These two cases can be seen below.=

![Screenshot of empty igv window](/Images/Heterozygous_SNV.png)

![Screenshot of empty igv window](/Images/Homozygous_SNV.png)

The top screenshot show the case of a heterzygous mutation, where the alt allele A is in about 50% of the reads, and the refernce allele G is in the other 50%. The lower screenshot shows a homozygous mutation where the alt allele C is in all the reads and the reference G is not seen at all

To get the exact percentage of reads with the alternatvie read click the base in the read depth track (in this case it is the green/orange rectangle above the A's and the blue rectangle above the C's respecticly)

In addition you may see a case like the following

![Screenshot of empty igv window](/Images/Somatic_SNV.png)

while the signal is a bit messy here (which is common with real data) we can see the majority of the reads match the refenence. However there is a signficant amount of reads that show a C at this base instead of a T. There could be a few explinations for how this occured. Firsly this could be a heterozygous mutations, however the probably of only 14 or less of the 68 reads being the alternate allele are 0.00000042, which is highly unlikely. It could be sequencing error as well, however SRS has a relitivly low error rate of about 1% (depending on the sequencing tool), so the odds of the same sequencing error occuring 14 times in 68 reads is again extremly low. Thus the most likely explination is it is a somatic mutation, or a mutation that is not in all cells. There is no hard cutoff percentige for somatic vs heterozygous vs homozygous so you will need to decide beforehand how to will classify them.

### challenge
how would you label the following mutations in `WGS_001_Father`. All the following mutations can be found in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_snvs_chr1.vcf`

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
3. Somatic (37% of the reads showing the alt allele in a heterouzygous is unlikely, but very possible. Depending on your criteria a clasifction of heterozugous or somatic could be appopriate)
4. Heterozygous
5. heteroygous and somatic (the T-> Mutation appears to be heterozygous and the Y->G mutatation looks somatic being in 3% of reads)
6. Homozygous
7. mutliallelic heterozygous(one chromosome is A->C, the other is A->T)
</details>

# Identifying small indels in IGV 
Indels can be broken down into insertions and deletions. Most of the time indels are able to be clearly seen in IGV. However there may be some edge cases that are more difficult to be able to see. we will go over all these cases the the section below. In addition there are a few settings that must be on to detect the indels. Like with SNVs `Show mismatched bases` must be on. In addition make sure `label indels > theshold` checked and `Label threshold (bases)` set to 0. Finally concerm that `shows soft-clipped bases` is check. For the last two setting they can be found in `view->preferences->alignments`

## insetions
insertions occurer when there are new bases inserted between two bases in the reference sequence. These can be seen in IGV by looking for purple bars with white numbers on them in the reads (see below). The location in the read will always be between two bases, showing when the seuqnece is instered into the DNA, and the number rep[resents how many bases are insterted. To see the exact seqeunce instered click on the purple bar

![Screenshot of empty igv window](/Images/indel_insertions.png)

## Deletions
Deletions occure when a small sequence of DNA is removed compared to the reference sequence. Like with indels IGV will directly show these on the reads. In this case a part fo the read will be replaced with a black line with a purple number. That number again shows the number of bases missing. To get the sequence missing you can look at the sequence track and see what bases the gap overlaps

![Screenshot of empty igv window](/Images/indel_deletion.png)

Like with SNVs, indels can be catadoaired as heterozygous, homozygous, or somatic, based on the AF. However unlike with SNVs these is no easy way to get the exact number of reads that show the mutation without manually counting it. For deletions you could compare the read depth outside the mutation vs inside the mutations, but these values are subject to noise and misaligment making them not the most reliable. In addition keep in mind that indels in multiple locations could be the same indel if the regeon is a repeat.

![Screenshot of empty igv window](/Images/indel_repeat_regeion.png)

In this screenshot IGV shows a deletion of the sequence `TTTTTC` from chr1:155568828-155568833. However this region is a repeat of that `TTTTTC` sequence, so it is imporible to know which repeat is missing. Thus if the deletion was labeled from chr1:155568834-155568839 it would be refering to the same indel

Finally for larger indels (by defenution indels cannot be larger than 50 bases otherwise they would be catoigrozied as a structual variant) is is possible for a read to not span the entire mutation. This prevent IGV from showing the SV. instead you will see soft clips at the end of the read.

![Screenshot of empty igv window](/Images/indel_del_with_soft_clips.png)
![Screenshot of empty igv window](/Images/indel_ins_with_soft_clips.png)

In these two examples we can see most of the reads label the indel. Howeverm some of the reads just shiow a sequence of mismatched bases at the end. This is a soft clip. With IGV is saying is up to a point this read is aligning with the reference sequence here, but at the soft clip it is no longer a match. For the deletion we can see the soft clip sequence (the bases in the read) corresponds with the sequnce after the labeled indel. This matches with the other reads showing the indel as all the reads show the consesnus sequence with those 6 bases missing. IGV is just unabel to label the indel on these reads because it does not have enough of the sequence following to be sure the soft clip seqeucne comes from that region. In the insertions we can see it is an sertion of the bases `CA`. However IGV shows more than those bases in the soft clip. However looking at the soft clip seqeucne we see the `CA` following the expected reference sequence before the indel. This matches with the other indels as it shows the reference seqeunce before the indel, followed by the indel lequence, and then the referecne sequence after the indel (sequence after is not shown since it is aigming with the reference) Again it is shown as a soft clip rather than an indel since IGV does not have enough of the sequence before the indel to be sure that is where the sequence is from. Keep this in mind if you are visuaklizing indels and you see soft clips rather than indel markers.

### challenge
how would you label the following mutations in `WGS_001_Father`. All the following mutations can be found in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Father_UDN121697_indels_chr1.vcf`

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
Strucutral variants are variants impacting more than 50 bases. thay are the most complicated to detect in IGV because most sequencing is short read, and thus do not span the entire read. Therefore IGV is unable to directly show you the mutation, and rather you must use clue to infer the presense of an SV. Thus this guide will go over the major SV's you may run into along with how they will appear in IGV. For more detail, including how visualze comxplex stuctual variants, refer to the following slides. ([Visualize common SVs](https://docs.google.com/presentation/d/13rQ7rrYeOpSAon3BPFgQPzYyYhBAPeLo_uVq0ET3s7E/edit?usp=sharing), [Visualize complex SVs](https://docs.google.com/presentation/d/1dCcq4Rq3-AJASmq7eK62l8YLsMMCMNWzQjSJYnHTcgo/edit?usp=sharing))

## Overview of evidence
All the peices of evidence you will want to use have been mentioned before in this guide but here we will consolidate it to one place. For a visual example of each piece of evidence refer to the image below

![Screenshot of empty igv window](/Images/IGV_Evidence.png)

### Read Depth
Read depth is the number of reads that cover an indivudalze base. Any read that align over a base will increase the read depth unless it is a soft clip at that location. The read depth is shown in IGV as the grey bar at the top of bam file track. For variants that either duplicate or deleate regions of the genome there will be a change in the read depth that match the genomic contect getting added or removed. However read depth may not always be a sharp change such as in the image above. In a repeat or duplicated region a read spaning deleted/duplicated region may look very similar to if the variant was not there. This tends to result is a gradual increase or decrease in the read depth 
### Soft Clips
Soft clips, also known as split reads, are parts of a indivudual read that have the genomic contect of a different location. This can occure when a read spans the breakpoint, or boundry, of a variant. This makes soft clips very usful in idenfying the exact coordinates of a variant. However, like with read depth. of the variant is in a duplicate or repeat region there may not be a soft clip. As it is valuable ti know where the soft clip sequence is from BLAT is a very powerful tool in understanding soft clips.
### Insert Size
Insert size refers to the distance between the two paired reads in short read seqeuncing. For most SRS, this value tends to be around 300 bases, give or take 100 bases. However where there is a structural variant in the region of the read, it can result in the distance between the two reads, when aligned to the reference, being much larger or small than expect. In a read in labled in blue it means this distance is smaller that expect, and when it is red it is larger than expected. In addition, if the two reads align to different chromosome instead of bing clolred red or blue, it will be colored based on the chromosome of the pair.
### Pair-end Orientation
in Short read sequencing the first read is sequence from 5' to 3' and the second read is sequence 3' ot 5'. A read in the 5' to 3' direction is refered to as being in the left direction (don't ask why), and the 3' to 5' read is in the right direction, meaning the pair together is LR. This is the pair end orinetation. However large variants can chnage this orientation, resulting in RL, LL and RR reads. These are label as green, cyan, and blue respectivly

With these four peice of evidence established we will now go into detail into the main five kinds of variants, deletions, duplications, inversion, insertions, and translocation. It is imprtant to not just understand the evidence but understand why the evidence looks this way so make sure to spend time understanding how the signal occurs.

## Deletion
A deletion is a remove of a large segment of the genome. They tend to be easier to detect as the evidence to see a deletion is staightforward. You want to look for three main peices of evidence. First is a drop in the read depth. This drop would likely be 50% of the read depth around for a heterozygous mutation and 100% of the read depth for a homozygous mutation. Second is insert size. If the two reads span the deletion, there is more genomic infomation between the reads when aligning to the reference, resulting in a insert size larger than expected, or them being label red. Finally there are soft clips. the soft clips should be inward and show the seqeunce at the other soft clip.

![Screenshot of empty igv window](/Images/SV_DEL.png)

In the image above note the blat alignment at the bottom shows the Soft clip sequence by the left soft clip coresponds with ref sequence by right soft clip and vice versa. In addition, doing a blat aligment of the entire read with the soft clip show the gap. This evidence, with the red Insert size reads and complete loss in read depth, conferms this is a homozygous deletion

## Duplications
unsuprisingly, a duplicated region of the genome in IGV looks like the opposite of a deletion. You should see an increase in the read depth are there are more reads from the udplicated regeion. You should also see blue insertisze lables showing the reads are close than expected as the duplicated regions between two reads is not in the reference, resukting in the two reads being closer. You will aslo see outward soft clips of the other breakpoint corresponding with reads spanning the boundtry between the two duplicated regions. The  only additional peice of evidence is green pair end orientations showing RL oritentions. This occures because if a read is from the end of the first duplication and itsd pair sequrnces the start of the second duplication, the second read will map to beofre the first read. All this evidence together could look like the following

![Screenshot of empty igv window](/Images/IGV_Evidence.png)

it is important to note most duplications, and in fact most variants, will not show all the posible evidence. For isntace is the duplication is too small there will not be pair end orinetation. Or if the duplication is too big the reafs wont be able to span the duplication, so there wont be insert size evidence. It is important to consider the evidence pressent in IGV and see which, if any, of the variant types match the evidence the best.

## Inversion
An inversion is when a region of the genome flips directions. Thus the end of the region becomes the start and vise versa. An important consiquence of this is since the 3' end goes to the 5; end, but the stand is still 5' to 3'. it will flip to the compe,entay starnd. As there is not genomic information gained or lost there sdhould not be a read depth chnage. They could be inser size evidence based on how big the inversion is, meaning the reads could be blue or red. The most imnportsnt evdidece is soft clips on both sides of the break points and RR and LL pair end orientation. However since inversion most comply occure from crossover between similar sequences in the genome, soft clips may not always occur

![Screenshot of empty igv window](/Images/SV_INV.png)

In this case there are no break point outward at the first breakpoint. There is also a chnage in the read depth in this region. Abnormalizties like these are common in inversions, however the LL and RR reads, along with the soft clip sequences being found in the nearby region is enough to be sure this is an inversions

## Translocation
A translocation is a swap between genomic content in two chromosome. They are the only<sup>*</sup> variant where is igv you will only see one breakpoint. At this breakpoint you should see soft clips om both sides. Finally there should be many reads who's mate is maped to a different chomosome, and this should all be the same chromosome. In addition, since this impacts two chromosomes you should see similat signal at the other chrosmosme.

![Screenshot of empty igv window](/Images/SV_CTX_1.png)
![Screenshot of empty igv window](/Images/SV_CTX_2.png)

Note we need two images. One from chromosome 12, one from chomosome 13. In each of these we see the same color for the reads, the ones in chr12 are the color for chr13 and vise versa. This is the defining singal for a translocation. Also note this is not a screenshot from WGS001_Father like everything prior. This is because translocations are extremmly rare. This is the only sample I have access to (out ouf 100+) that I knew had a translocation. Keep this in mind if you thing you see a translocation or are trying to conferm one.

## Insertion
We have saved insertions for last because they are the most complex to see. Insertions are added bases to a chromosome, either from elsewere in the genome or are random sequences. The complexity comes from multiple mechanisms that case reuslt in insertions and how the signal can look very similar to other variants. In general the read depth will not change in an insertions. However based on the mechansims there can also be a deletion of a few bases or duplication of an insert site. However these then to be small, for sure less than 50 bases. Thus if you see a read deth chnage of less than 50 bases thjink insertions, however the lact of a read depth chnage could also me insetion. The pair end orientation and insert size can also be anything based on where the inserted sequence is from and how large it is. In general this evidence is harder to use for insertions. Finally you will see soft clips om both side of the break point(s). Thus insertions can look like any of the following

![Screenshot of empty igv window](/Images/SV_INS_RANDOM.png)
![Screenshot of empty igv window](/Images/SV_INS_DEL.png)
![Screenshot of empty igv window](/Images/SV_CTX_2.png)

Note the seauence `GCCTGAAATGATCTTCCTCCGTCTTACCTATGAACTGGTCCTATCTTCCCTCTTCATTCACAAATATTCATTAGGTGC` that is present in both soft clips of the first screenshot. This is the inserted sequence. Looking at the reads and removing this sequence you will see it matches the reference seqeunce with a duplciatetion of the insert size (which is why we see the read depth increase) This is the classic signal for an insertion of a random sequence. It can be confermed the inserted sequence is random since trying to blat the soft clip will have no strong matches

the seciond screenshot is the same case as the first, however instead of a insert size tuplication a few bases have been deleted. This is likely due to non-homologous end joining but is another case of an insertions

The thrid screenshot is the classic example of a mobile element insertion. MEI alwasy have insert site duplications and tend to have reads whos mate maps to a different chromosome but not a consitent chromosome. This is becuase they are each mapping to a different instance of the same tranposible lement thought the genome

![Screenshot of empty igv window](/Images/SV_INS_1_12_image_1.png)
![Screenshot of empty igv window](/Images/SV_INS_1_12_image_2.png)

These last two images show the same insertion. Note the conistent color of the reads showing their mate maps to a diffent chrmosome. In the first image these are maping to chr12. Going to this region in chr12 we see the second image, where they map to chr1. 
In the second image however it does not look like an insertion at the breakpoints are far away from eachother. This is how we know it is an insertion and not a translocation. the region between the two break points in chr12 has been inserted in chr1

## False Positives
Due to short read sequencings difficulty with SV detection it can have a high false positive rate. Thus it is important to know how false positives can occure. There are two main situations

False psotive usally a cause by repedtive regions of the genome. The conistent way to know it is a false positive if by looking at the soft clips. If it is a true positive the soft clip should be consitent for all the reads. However in these two images we don't see a consitent sequence. This means these are false positives and should be distcarded

# QUIZ TIME!
It can be very difficult to understand how to visualzie variants without proacice, so go though the 20 cases in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.practice.vcf`. These all represent regions in `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.bam`. Label them are true positives or false positves, and if they are true positives label what variant class they are (SNV, indel, or SV), how frequent they are in the genome (somatic, heteroygous, or homozygous) and for indels and SVs what kind they are (insertions, deletion, duplication, inversion, or translocation). 

Two  notes about labeling
-  a somatic variant will be one in less than 30% of reads
-  a indel will be 50 bases or less, with a sturcutal variant being 51 bases or more
Keep in mind these defentions can change based on the project so don't use these thereshold exactly going forward. This is just to provide coinsitnct in how we label the variants

After analysing those 20 variants go to `/storage1/fs1/jin810/Active/testing/Ruttenberg/SideProjects/IGV_Tutorial/WGS_001_Mother_UDN613923.answerkey.vcf` to see the answer. feel free to check in with Andrew if you have any discrpecencies between your labeling and the answer key.



