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

***Step A. Install local NCBI BLAST+ on Palmetto.***

* Open a terminal and log into a Palmetto headnode.
Get an interactive node on Palmetto.  Here is an example SLURm command to get an interactive node:

```
srun --nodes=1 --ntasks-per-node=1 --time=02:00:00 --pty bash -i
```

Download the BLAST executable.
```
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.11.0/ncbi-blast-2.11.0+-x64-linux.tar.gz
```
Uncompress the BLAST tarball and copy files into your path.
Uncompress that tarball (a directory of files that have been double compressed with tar and gzip applications). 

Make a directory called ‘bin’ in your home directory. 

```
mkdir ~/bin
```
Go into the BLAST bin directory:

```
cd ~/ncbi-blast-2.11.0+/bin
```

Copy all the binaries here to the ‘~/bin’ directory you made in your home directory.  This will put all of the BLAST programs in your path. 

If everything worked, you can run BLAST commands like:

```
makeblastdb -h
```
Download the sequence files and index the BLAST database.

# Task B

***Step A. Download the human genome and cDNA sequence files into a directory in scratch.

Open a terminal and log into Palmetto.
Get an Interactive node on Palmetto.
```
srun --nodes=1 --ntasks-per-node=1 --time=04:00:00 --pty bash -i
```
Make a directory to do do this project in scratch.

```
EXAMPLE: /scratch/<your_username>/blast
```

Download the files you will need to this directory.

Here are the relevant links:
```
http://ftp.ensembl.org/pub/release-101/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
ftp://ftp.ensembl.org/pub/release-101/fasta/homo_sapiens/ncrna/Homo_sapiens.GRCh38.ncrna.fa.gz
```

Uncompress the files and ‘head’ and ‘tail’ them on the Linux command line to see if they look correct.

***Step B. Index the Genome file for BLAST

Use a text editor (e.g. nano) to make a SLURM script (e.g. index.pbs) with the commands you need.  The top of the SLURM script are SLURM directives that tell the scheduler how many resources you need.  Below the directives, add the command lines you will need to index the FASTA genome file for BLAST. You will need to change to the directory where you will index the files and then run the makeblastdb command on the genome file. Make sure to use your username and not mine. For example (make sure each command line is on one line in your SLURM script file):
```
#!/bin/bash
#SBATCH --job-name MAKEBLASTDB
#SBATCH --nodes 1
#SBATCH --tasks-per-node 1
#SBATCH --cpus-per-task 1
#SBATCH --mem 64gb
#SBATCH --time 24:00:00

cd /scratch/ffeltus/blast
makeblastdb -in Homo_sapiens.GRCh38.dna.primary_assembly.fa -dbtype nucl -out HG38_GENOME
```
Run the SLURM script using sbatch.

```
sbatch index.slurm
```

This will take a while. Use the squeue command to see if the job is still running.

```
squeue -u YOUR_USERNAME
```
If it worked you will see indexed files that start with HG38_GENOME in the scratch2 directory where the genome FASTA file is located.

