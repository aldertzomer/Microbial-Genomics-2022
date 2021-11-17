---
title: "Sequence Quality"
exercises: 60
questions:
- "What is the N50 ?"
- "What are single copy chromosomal marker genes ?"
objectives:
- "Learning to use assembly quality assessment tools"
keypoints:
- "Quality of a genome assembly can be assessed by looking at some basic statistics on the assembly, but also by using an external reference"
---

To investigate if your sequenced genome is of sufficient quality to be used, various statistics on the genome assembly can be gathered. Some are simple (e.g. number of contigs, genome sizes, N50), some require the use of a reference genome or reference genes.

## Basic statistics
In the previous lesson you have investigated the number of contigs. Other statistics include genome size and the N50. 

### Genome size

Getting the genome size is basically just counting the bases in a fasta file and excluding the fasta header (containing ">"). Unfortunately the end of line character ("\n") needs to be removed as well as else this character will be counted as well. We use the GNU tool "tr" for that.  

~~~
$ cd ~/assembly/ERR326690
$ cat scaffolds.fasta |grep -v ">" |tr -d "\n" | wc
~~~
{: .bash}

Questions: What does the -v option do in the grep command? And what does the wc command do?

### Generating the N50

From wikipedia:
N50 can be described as a weighted median statistic such that 50% of the entire assembly is contained in contigs or scaffolds equal to or larger than this value. Given a set of contigs, each with its own length, the N50 length is defined as the shortest sequence length at 50% of the genome. The N50 is similar to a mean or median of lengths, but has greater weight given to the longer contigs. The N75 is defined as the shortest sequence length at 75% of the genome.

It can be thought of as the point of half of the mass of the distribution; the number of bases from all contigs longer than the N50 will be close to the number of bases from all contigs shorter than the N50. For example, consider 9 contigs with the lengths 2,3,4,5,6,7,8,9,and 10; their sum is 54, half of the sum is 27, and the size of the genome also happens to be 54. 50% of this assembly would be 10 + 9 + 8 = 27 (half the length of the sequence, starting from the largest). Thus the N50=8, which is the size of the contig which, along with the larger contigs, contain half of sequence of a particular genome. Note: When comparing N50 values from different assemblies, the genomes must have similar sizes.

We can generate the N50, the N75 or N90 by hand, but we will also be making use of the tool called QUAST ( http://quast.sourceforge.net/quast ). The following Excel spreadsheet can help you generate it by hand [https://github.com/aldertzomer/Microbial-Genomics-2021/raw/gh-pages/files/How%20to%20calculate%20N50_N80_N90.xlsx](https://github.com/aldertzomer/Microbial-Genomics-2021/raw/gh-pages/files/How%20to%20calculate%20N50_N80_N90.xlsx).

Quast can be run by the following commandline and will generate a folder called quast_ERR326690 containing interesting statistics. Read the file report.txt.
~~~
$ quast.py scaffolds.fasta -o quast_ERR326690 --min-contig 0
~~~
{: .bash}

The contig sizes from an assembly by SPAdes can be obtained by:
~~~
$ cat scaffolds.fasta  |grep ">" |cut -f 4 -d _
~~~
{: .bash}


Access the folder using your webbrowser and go to the right folder. 


#### Question 1: Determine the N50 and number of contigs by hand and by using the QUAST tool with and without the --min-contig option. Is there a difference? why? 

#### Question 2: Think of a few situations where comparing the genome size, number of contigs or N50 would be less useful




### Assessing the quality using single copy chromosomal marker genes.

An excellent way to assess if your genome is not missing any sequence or is not contaminated is checking if all single copy chromosomal marker genes are there and what their copy number is. We will be using the tool called CheckM ( https://ecogenomics.github.io/CheckM/ ). CheckM is not very fast as it needs to detect 1000s of single copy chromosomal marker genes.

First we need to figure out which species/genera/families are available as single copy chromosomal marker genes:

~~~
$ 
$ checkm taxon_list > taxonlist.txt
~~~
{: .bash}

Obviously we want to select the closest match as possible. In the case of E. coli, we would choose "Escherichia coli". 


CheckM can be run by the following commandline in the case of an E. coli genome called genome.fasta in the current folder (".") using the following command line. All output is stored in the folder checkmout and a useful table is checkmoutput.tsv. 
~~~
$ 
$ checkm taxonomy_wf species "Escherichia coli" . checkmout -t 1 -x genome.fasta  >checkmoutput.tsv
~~~
{: .bash}

It is possible to run the commmand on multiple genomes for faster analyses in the folder called "genomes" on all files with the extension (-x) .fasta:

~~~
$ checkm taxonomy_wf species "Escherichia coli" genomes checkmout -t 1 -x fasta  >checkmoutput.tsv
~~~
{: .bash}

The file checkmoutput.tsv contains three relevant outputs: completeness, contamination and heterogeneity. Completeness is how complete your genome is, contamination lists if there is another species present, heterogeneity lists if there is possibly contamination with a different strain of the same species (ignore this value in most species).

> ## Challenge: What is the quality of your genome assemblies
>
>  Determine the number of contigs, assembly sizes, N50, completeness and heterogeneity for your genomes and enter these in this
> [table](https://docs.google.com/spreadsheets/d/1b8BPKcSUuW2YzgHdMaJN3MEbdgroRJa1dWnf5gkHr9M/edit#gid=0)
>
> Hint:
> ~~~
> $ wc, quast.py and checkm
> ~~~
> 
> 
> > ## Solution
> >
> > 
> > ~~~
> > $ mkdir ~/genomes
> > $ cp ~/assembly/ERR326690/scaffolds.fasta ~/genomes/ERR326690.fasta
> > $ cp ~/assembly/ERR326694/scaffolds.fasta ~/genomes/ERR326694.fasta
> > $ cd ~/genomes/
> > $ quast.py ERR326690.fasta -o quast_ERR326690 --min-contig 0
> > Open report.html in web browser
> >
> > $ checkm taxon_list |grep Streptococcus
> > ....
> > species   Streptococcus pneumoniae                                20           846              160
> > $ cd ~/genomes/
> > # Analyse a single file in the current folder (the .) that matches ERR326690.fasta. This does not work very well in a loop. 
> > $ checkm taxonomy_wf species "Streptococcus pneumoniae" . checkmout -t 1 -x ERR326690.fasta  >checkmoutput.tsv
> >
> > # Alternatively, process all genomes in the "genomes" folder that have the extension fasta. This is useful if you have many genomes to check. 
> > $ cd ~
> > $ checkm taxonomy_wf species "Streptococcus pneumoniae" genomes checkmout -t 1 -x fasta  > checkmoutput.tsv
> > $ cat checkmoutput.tsv
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

