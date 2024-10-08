# lab-scaling-local-blast-experiments-on-the-palmetto-supercomputer

# Lab Overview
The Basic Local Alignment Search Tool (BLAST) will perform pairwise alignments using the word-based BLAST search algorithm: Seed, Search, Score, Extend.  The NCBI website is great for a few BLAST searches, but a local install of BLAST on a Linux machine allows one to scale up and control the BLAST experiment. 

Running BLAST on a supercomputer allows for big time scale up.  In this lab you will run a BLAST job that can take more than 24 hours. In this lab we will install BLAST+ on Palmetto and use it to map all human non-coding RNAs to the hg38 human reference genome. 

Don’t forget to review how to use Palmetto here:

https://docs.rcd.clemson.edu/palmetto/

# Lab Objectives
..*Install BLAST+ on the Palmetto Cluster
*Format a sequence database on the local machine.
*Perform a BLAST experiment using a PBS batch job.

