---
title: "Inspecting sequence graphs"
teaching: 20
exercises: 70
questions:
- "Can we find out which scaffolds or contigs are connected?"
objectives:
- "Find a 6 kb scaffold with high coverage"
- "Figure out the copy number"
- "To which scaffolds are the copies of this scaffold connected "
keypoints:
- "A genome assembly is fragmented because of repeats in the genome. The assembly graph display possible connections between contigs."
---

## Introducing the sequence graph

In the assembly lecture the concept of a sequence graph was introduced. It is possible to view this graph using the tool Bandage [https://rrwick.github.io/Bandage/](https://rrwick.github.io/Bandage/). You can download the file in the link, unzip it, and follow the installation instructions. 

### Examining the file containing the sequence graph

View the sequence graph using less:

~~~
$ cd ~/assemblies/ERR326690/
$ ls
$ less -S assembly_graph.fastg
>EDGE_1_length_4276_cov_92.8356:EDGE_743_length_70_cov_235.8';
AAATTAATTTGACTTTCCTGATAGAGTTGTTCACATCTTATTTCAATCTACTATATTTTA
TAGAACAGACTACTCTGAAAGTAGTTTCAGACCTCTTATGATTTCGTATCAGCCTGAATG
TCATCAAAAAAAGATAGCAGGCTTAAAAACCTGCTATCTCCTTCTATTTTTACAAAATCA
~~~
{: .bash}

Can you understand how the sequence graph works? Look at the header:
~~~
$ cat assembly_graph.fastg |grep ">"

EDGE_1_length_4276_cov_92.8356:EDGE_743_length_70_cov_235.8';
EDGE_1_length_4276_cov_92.8356':EDGE_2_length_62_cov_265.143;
EDGE_2_length_62_cov_265.143:EDGE_567_length_57_cov_188',EDGE_820_length_34692_cov_92.9214';
EDGE_2_length_62_cov_265.143':EDGE_1_length_4276_cov_92.8356,EDGE_424_length_250_cov_165.728';
EDGE_3_length_73_cov_163.333:EDGE_572_length_1655_cov_89.6863,EDGE_573_length_10177_cov_90.9533;
EDGE_3_length_73_cov_163.333':EDGE_283_length_8100_cov_90.431',EDGE_895_length_70_cov_83.8';
EDGE_4_length_221_cov_0.801205;
EDGE_4_length_221_cov_0.801205';
~~~
{: .bash}

###  Visualizing the sequence graph

Download a the sequence graph from your assembly. Open it in Bandage. Click on Draw graph. What does this remind you of?
Click on a few large contigs. What is the average coverage? 

### Selecting a contig with high coverage

Now, we will select a contig which has a 3x higher coverage. Click on Scope, select depth range. Fill in for min coverage 100 and max coverage 1000  (or other values, around 3x the average contig depth). Press Draw Graph. What contigs are now displayed? Why do these have a higher coverage? 

### Investigating a contig with high coverage using Bandage and BLAST

Click on the largest contig, should be around 5-6 kb. What do you think this contig is?  In the "Output" menu, at the top, the sequence of the selected contig can be copied to the clipboard. Figure out what the contig codes for using BLASTN [https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome).  Also record the contig number.

### Where are these sequences located?

Although the sequence is a separate contig in the fasta file, we can figure out to which contigs it is connected. We could for instance investigate the assembly_graph.fastg in a text editor. Alternatively, we select scope "Around nodes" and fill in the number of our selected contig. Next, we fill in 4 for the distance and press Draw Graph. What do you think this does? Try to figure out where the contig of interest is connected to.  Name another situation where an assembly graph could be useful for (Hint: Plasmids..)


