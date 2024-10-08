# lab-scaling-local-blast-experiments-on-the-palmetto-supercomputer

# Lab Overview
The Basic Local Alignment Search Tool (BLAST) will perform pairwise alignments using the word-based BLAST search algorithm: Seed, Search, Score, Extend.  The NCBI website is great for a few BLAST searches, but a local install of BLAST on a Linux machine allows one to scale up and control the BLAST experiment. 

Running BLAST on a supercomputer allows for big time scale up.  In this lab you will run a BLAST job that can take more than 24 hours. In this lab we will install BLAST+ on Palmetto and use it to map all human non-coding RNAs to the hg38 human reference genome. 

Don’t forget to review how to use Palmetto here:

https://docs.rcd.clemson.edu/palmetto/

# Lab Objectives
* Install BLAST+ on the Palmetto Cluster
* Format a sequence database on the local machine.
* Perform a BLAST experiment using a SLURM batch job.

# Task A

Install BLAST+ on Palmetto so you can run massive BLAST jobs.

# Step A. Install local NCBI BLAST+ on Palmetto.

Open a terminal and log into Palmetto.
Get an interactive node on Palmetto like this:

'qsub -I -l select=1:ncpus=2:mem=4gb,walltime=24:00:00
Download the BLAST executable.
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.11.0/ncbi-blast-2.11.0+-x64-linux.tar.gz
Uncompress the BLAST tarball and copy files into your path.
Uncompress that tarball (a directory of files that have been double compressed with tar and gzip applications). Google is your friend (GIYF).

Make a directory called ‘bin’ in your home directory. 

mkdir ~/bin
Go into the BLAST bin directory:

cd ~/ncbi-blast-2.11.0+/bin
Copy all the binaries here to the ‘~/bin’ directory you made in your home directory.  This will put all of the BLAST programs in your path. 

If everything worked, you can run BLAST commands like:

makeblastdb -h

