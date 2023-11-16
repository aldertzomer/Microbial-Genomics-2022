---
title: "Introduction"
exercises: 10
teaching: 20
questions:
- "Where does the dataset come from?"
- "How to login"
- "Where are the files located"
objectives:
- "Understand the data"
- "Choose login details"
- "Familiarize yourself with the environment"
keypoints:
- "Sequencing *S. pneumoniae* patient isolates to determine assocations of bacterial genes with disease severity"
---

## Introduction

We will be making use of the files on the [Cocalc4 site](https://cocalc4.science.uu.nl/) or [Cocalc3 site](https://cocalc3.science.uu.nl/). 

### Dataset

Introduction to the dataset is given in the [presentation](files/Aldert Zomer Introduction lecture and GWAS lecture.pdf). In total we will be analyzing 62 genomes, of which one is a closed reference genome (OXC141).

### How to login

Follow the instructions at as before. 
The server we will be using has host address [https://cocalc4.science.uu.nl/](https://cocalc4.science.uu.nl/). Please login using your webbrowser. Click on "New", top left and open a "Linux Terminal". Give it an appropriate name so you can find it again later. 

### Where are the files located

In your home folder (~/), you may find different files. It is your own responsibility to take care of your files. We will create the folders you will be using and download the read files that are part of this study. As assembling of all the genomes in this study would be too time consuming, we will assembling only two genomes per person. We will combine the outputs of each person later on. Familiarize yourself a bit with the terminal in Cocalc.
  
### Getting the Illumina read files

First we need to make an appropriate folder for your read files. In the example we will be making use of the folder called "reads"

~~~
$ cd ~
$ mkdir reads
$ ls
~~~

You will see you have created the folder reads. Next we need to get the appropriate files from the server. Go to the website [klif.uu.nl/klif/mgen/reads](https://klif.uu.nl/klif/mgen/reads/) and download the appropriate files. You will need two files for each sample, the forward and reverse file. In the example I have picked the top two, but please take a look at the  the [Google Sheets table](https://docs.google.com/spreadsheets/d/1b8BPKcSUuW2YzgHdMaJN3MEbdgroRJa1dWnf5gkHr9M/edit#gid=0) and write your name in the appropriate field to find out which two samples are assigned to you. You will need to get four files, as each sample has a forward and reverse read file.

~~~
$ cd ~/reads
$ wget https://klif.uu.nl/klif/mgen/reads/ERR326690_1.fastq.gz
$ wget https://klif.uu.nl/klif/mgen/reads/ERR326690_2.fastq.gz
$ wget https://klif.uu.nl/klif/mgen/reads/ERR326694_1.fastq.gz
$ wget https://klif.uu.nl/klif/mgen/reads/ERR326694_2.fastq.gz
$ ls
~~~

In the above example we have downloaded the read files. Why are there two files per sample? Please continue on with the next part of the course which is a lecture on sequence quality of read files. 
