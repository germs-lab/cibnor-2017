---
layout: page
title: Running BLAST from the command line to identify environmental sequences
comments: true
date: 2015-06-25 14:00:00
---
# Running BLAST from the command line to identify environmental sequnences
Authored by Jin Choi for EDAMAME2015, based on a previous tutorial.  Modified by Adina Howe.
[EDAMAME-2015 wiki](https://github.com/edamame-course/2015-tutorials/wiki)

***
EDAMAME tutorials have a CC-BY [license](https://github.com/edamame-course/2015-tutorials/blob/master/LICENSE.md). _Share, adapt, and attribute please!_
***

## Overarching Goal
* This tutorial introduces users to installing a program and running it on the shell command line.  Specifically, this tutorial is applicable to users who would like to identify sequence homology of query sequences to a custom dataabase.

## Learning Objectives
* Repeat the web-online BLAST on the command line
* Install BLAST program from the command line
* Execute and automate a blast program
* Compare a sequence query to a sequence databsae
* Analyze results

***

# Introduction
Let's have a BLAST! That's right, the [Basic Local Alignment Search Tool](http://en.wikipedia.org/wiki/BLAST)!

You all may have previously used the NCBI BLAST Web page to do individual searches. Today we'll automate batch searches at the command line on your own computer. 

## Motiviation: Why would I want to run BLAST locally?
 
1. I want to BLAST against my own database (and maybe it's secret!)
2. I'm tired of waiting on the web interface to complete - it takes too long!

Today, imagine that you have genes that you have sequenced and want to identify the closest known sequences within a database.  

### First, let's install blast.

First, let's check and see if we have BLAST installed, and this will depend on the compute resources you are using.

```
which blastn
```

If you have BLAST installed and in your path (BLAST may be installed but not in your path), it will give you something like this:

```
/usr/bin/blastn
```

If you don't have BLAST, then you will need to install it.

## Step 1: Installing BLAST

To install the BLAST software, type the following command.  This installs the most recent version:

```
sudo apt-get install ncbi-blast+
```

## Step 2: Download the databases

Now, we can't run BLAST without creating a database to compare our sequences to. A popular database is to use all the genes in NCBI (e.g., similar to the web interface's databases).  This, like a lot of NCBI databases is huge, so I don't suggest putting this on your laptop unless you have a lot of room.  It's best on a larger computer (HPCC, Amazon machine).   For this tutorial, let's download a small database for this tutorial.  

We'll also download the sequences that you want to compare to your database.  This would be similar to obtaining sequences from your sequencing facility.

First, let's use curl to retrieve a database of genes.  These often come from a literature search or hard work.  In this tutorial, its easy though, you'll just download it...but assume you've searched the literature and these are genes of interest that your sequence might match:

```
curl -O https://s3.amazonaws.com/edamame/Blast_Tutorial.tar.gz
```

To get access to this data, let's unzip it.  It's in a compressed form to transfer from the "cloud" to your local computer faster.


```
tar -zxvf Blast_Tutorial.tar.gz
```

Next, let's navigate into the directory and take a look at the files it contains.

```
cd Blast_Tutorial
```

Now you've got these files. We can take a look at the size and names of this file with the following command...

```
ls -l
```

Here's some description of some these files:

MyQuery.txt:  This is actually (mis)labeled as a ".txt" file but it is a FASTA file.  How can you tell?   We're going to leave it as a ".txt" file, mainly because its very likely people will send you sequences like this in the future and its important to know that the extension of file type is somewhat arbitrary.

rep_set.fna: This is a database of previously observed genes.  You would like to know if your sequences match any of these genes which have been sequenced from the bacterial isolates in your lab.

So, now we've got the database files, but BLAST requires that each subject database be preformatted for use; this is a way of speeding up certain types of searches. To do this, we have to format the database.  You should do:

```
makeblastdb -in rep_set.fna -dbtype nucl -out rep_set.fna.db
```

The -in parameter gives the name of the database, the -out parameter says "save the results", and the -dbtype parameter says "what type of the database". For DNA, you'd want to use '-dbtype nucl'. FYI, for protein, '-dbtype prot'.

Let's see what the BLAST database looks like

```
ls
```

You may notice that there are 3 files that were generated.

rep_set.fna.db.nhr, rep_set.fna.db.nin, rep_set.fna.db.nsq

BLAST uses this 3 "indexed" files to make searching genes against genes go faster and more efficiently.

Just a reminder:

1. UNIX generally doesn't care what the file is called, so '.fna' and '.fa' are all the same to it. 
2. UNIX utilities work well with text files, and almost everything you'll encounter is a basic text file. This is different from Windows and Mac, where more complicated formats are used that can't be as easily dealt with on UNIX.

## Step 3: Run BLAST

Now try a BLAST: You need a file that have your query in.  Remember, here is your query looks like.

```
cat MyQuery.txt
```

This file contains sequences of 4 bacteria and 5 fungi that you have gotten sequenced.  You wonder if it matches anything in the known database.  

To do this, we'll run a BLAST. We use blastn because we will search a nucleotide database using a nucleotide query.

```
blastn -db rep_set.fna.db -query MyQuery.txt
```

Once you get the output, you can do a few different things...

1.  First, you can scroll up in your terminal window to look at the output.  
2.  Second, you can save the output to a file:

```
blastn -db rep_set.fna.db -query MyQuery.txt -out out.txt
```

and then use 'less' to look at it:

```
less out.txt
```

3.  You can pipe (|) it directly to less, by which I mean you can send all of the output to the 'less' command without saving it to a file:

```
blastn -db rep_set.fna.db -query MyQuery.txt | less
```

Sometimes tabular form (output format) is useful. To get the result in tabular form,

```
blastn -db rep_set.fna.db -query MyQuery.txt -out outtabular.txt -outfmt 6
```

Let's try a different blast so you get your own practice, with a slight twist.  Imagine you've compiled a database of genes from all isolates that originate from soil, and you would like to compare it to genes in NCBI RefSeq (a popular genome database).  We're providing you two files -- the RefSoil16s.fa file -- the sequences of 16S rRNA genes from soil isolates and the database -- rep_set_sub.fna that you have downloaded from NCBI.

## Excercise

Identify your query and database and complete a BLAST.

## Different BLAST options
BLAST has lots and lots and lots of options. Run 'blastn' by itself to see what they are. Some of the most useful ones are `-evalue`.

If you want to search protein, use 'blastp' instead of 'blastn'. 'blastx', 'tblastn', 'tblastx' also available.

Here are the options/flags available to this program:

0 = pairwise,

1 = query-anchored showing identities,

2 = query-anchored no identities,

3 = flat query-anchored, show identities,

4 = flat query-anchored, no identities,

5 = XML Blast output,

6 = tabular,

7 = tabular with comment lines,

8 = Text ASN.1,

9 = Binary ASN.1

10 = Comma-separated values

11 = BLAST archive format (ASN.1)

***
##Help and other resources
* [Manual of Blast+](http://www.ncbi.nlm.nih.gov/books/NBK279690/)

* FTP link for RefSeq DB : ftp://ftp.ncbi.nlm.nih.gov/refseq/release/

* [RefSeq site](http://www.ncbi.nlm.nih.gov/refseq/)

* [HMP site](http://www.hmpdacc.org)

* [Stript for get the best hit from BLAST search in tabular form](https://github.com/metajinomics/Get_best_hit_blast_tabular)

-----------------------------------------------
-----------------------------------------------
