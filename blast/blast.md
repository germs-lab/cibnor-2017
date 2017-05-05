---
layout: page
title: Running BLAST from the command line to identify environmental sequences
comments: true
date: 2015-06-25 14:00:00
---
# Running BLAST from the command line to identify environmental sequnences
Authored by Jin Choi for EDAMAME2015, based on a previous tutorial.
[EDAMAME-2015 wiki](https://github.com/edamame-course/2015-tutorials/wiki)

***
EDAMAME tutorials have a CC-BY [license](https://github.com/edamame-course/2015-tutorials/blob/master/LICENSE.md). _Share, adapt, and attribute please!_
***

## Overarching Goal
* This tutorial will contribute towards an understanding of BLAST running locally

## Learning Objectives
* Build local DB
* Blast query into local DB

***

# Introduction
OK, your first introduction to the use and abuse of command line tools is... BLAST! That's right, the [Basic Local Alignment Search Tool](http://en.wikipedia.org/wiki/BLAST)!

Let's assume that all of you have used the NCBI BLAST Web page to do individual searches. Today we'll automate batch searches at the command line on your own computer. This is a technique that works well for small-to-medium sized sequencing data sets. The various database (nr, nt) are getting big enough that it's reasonably time consuming to search them on your own, although of course you can do it if you want – you might just have to wait a while for things to finish.

## Motiviation: Why I want to run BLAST locally?
 
1. BLAST on my own database (and maybe it's secret!)
2. Automation: run on server, downstream analysis, can be part of other program
3. Speed

Today, imagine that you have genes that you have sequenced and want to identify the closest known sequences within a database.  

### First, let's install blast.

First, let's check and see if we have BLAST.

```
which blastn
```

If you have BLAST installed and in your path (BLAST may be installed but not in your path), it will give you something like this:

```
/usr/bin/blastn
```

If you don't have BLAST, then you will need to install it.

## Step 1: Installing BLAST

To install the BLAST software, type this:

```
sudo apt-get install ncbi-blast+
```

## Step 2: Download the databases

Now, we can't run BLAST without creating a database to compare our sequences to. A popular database is to use all the genes in NCBI (e.g., similar to the web interface's databases).  This, like a lot of NCBI databases is huge, so I don't suggest putting this on your laptop unless you have a lot of room.  It's best on a larger computer (HPCC, Amazon machine).   For this tutorial, let's download small database for this tutorial.  We'll also download the sequences that you want to compare to your database.

Use curl to retrieve database and the file that we use today:

```
curl -O https://s3.amazonaws.com/edamame/Blast_Tutorial.tar.gz
```

unzip file.

```
tar -zxvf Blast_Tutorial.tar.gz
```

Let's navigate into the directory

```
cd Blast_Tutorial
```

Now you've got these files. How big are they?

```
ls -l
```

Let me tell you what are those files.

MyQuery.txt:  This is actually mislabeled as a ".txt" file but it is a FASTA file.  How can you tell?   We're going to leave it as a ".txt" file, mainly because its very likely people will send you sequences like this in the future and its important to know that the extension of file type is somewhat arbitrary.

rep_set.fna: This is a database of previously observed genes.  You would like to know if your sequences match any of these genes which have been sequenced from the bacterial isolates in your lab.


So, now we've got the database files, but BLAST requires that each subject database be preformatted for use; this is a way of speeding up certain types of searches. To do this, we have to format the database.  You should do:

```
makeblastdb -in rep_set.fna -dbtype nucl -out rep_set.fna.db
```

The -in parameter gives the name of the database, the -out parameter says "save the results", and the -dbtype parameter says "what type of the database". For DNA, you'd want to use '-dbtype nucl'. FYI, for protein, '-dbtype prot'.

Let's see how BLAST database looks like

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
Now try a BLAST: You need a file that have your query in. Here is your query looks like.

```
cat MyQuery.txt
```

We have 4 bacteria and 5 fungi.

Now BLAST. We use blastn because we will search nucleotide database using a nucleotide query.

```
blastn -db rep_set.fna.db -query MyQuery.txt
```

You can do three things at this point.

First, you can scroll up in your terminal window to look at the output.  

Second, you can save the output to a file:

```
blastn -db rep_set.fna.db -query MyQuery.txt -out out.txt
```

and then use 'less' to look at it:

```
less out.txt
```

and third, you can pipe it directly to less, by which I mean you can send all of the output to the 'less' command without saving it to a file:

```
blastn -db rep_set.fna.db -query MyQuery.txt | less
```

Sometimes tabular form (output format) is useful. To get the result in tabular form,

```
blastn -db rep_set.fna.db -query MyQuery.txt -out outtabular.txt -outfmt 6
```

## Excercise

Let's try to search Reference Soil database using a your 16s amplicon query.

### 1. Make Reference Soil database

Hint: Here is the fasta format of a reference soil database: RefSoil16s.fa

### 2. Search using a sample 16s amplicon query

Hint: You can use this sample query: rep_set_sub.fna

## Different BLAST options
BLAST has lots and lots and lots of options. Run 'blastn' by itself to see what they are. Some of the most useful ones are `-evalue`.

If you want to search protein, use 'blastp' instead of 'blastn'. 'blastx', 'tblastn', 'tblastx' also available.

alignment view options:

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
