# lab-scaling-local-blast-experiments-on-the-palmetto-supercomputer

# Lab Overview
The Basic Local Alignment Search Tool (BLAST) will perform pairwise alignments using the word-based BLAST search algorithm: Seed, Search, Score, Extend.  The NCBI website is great for a few BLAST searches, but a local install of BLAST on a Linux machine allows one to scale up and control the BLAST experiment. 

Running BLAST on a supercomputer allows for big time scale up.  In this lab you will run a BLAST job that can take more than 24 hours. In this lab we will install BLAST+ on Palmetto and use it to map all human non-coding RNAs to the hg38 human reference genome. 

# Getting help

Review how to use Palmetto here: https://docs.rcd.clemson.edu/palmetto/

Useful Prometheus prompts:

```
* Provide a brief overview of the BLAST local alignment algorithm and explain how the BLAST algorithm is different from a global alignment algorithm. 
* What is a job scheduler on a shared computer cluster and what is the scheduler on the Clemson Palmetto 2 cluster? 
* Please provide a cheatsheet of SLURM directives and command line tools.
* How do I create a text file on the bash command line?
```

# Lab Objectives
* Install BLAST+ on the Palmetto Cluster
* Format a sequence database on the local machine.
* Perform a BLAST experiment using a SLURM batch job.

# Task A

***Step A. Install local NCBI BLAST+ on Palmetto.***

* Open a terminal and log into a Palmetto headnode.
Get an interactive node on Palmetto.  Here is an example SLURM command to get an interactive node:

```
srun --nodes=1 --ntasks-per-node=1 --time=02:00:00 --pty bash -i
```

Create a working directory on scratch and go to it.

```
mkdir /scratch/ffeltus/blast-software
cd /scratch/ffeltus/blast-software
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

# Task B Index the human reference genomesequences for BLAST

***Step A. Download the human genome and cDNA sequence files into a directory into your working directory on scratch.***

Open a terminal and log into Palmetto.
Get an Interactive node on Palmetto.
```
srun --nodes=1 --ntasks-per-node=1 --time=04:00:00 --pty bash -i
```
Make a directory to do do this project in scratch.

```
mkdir /scratch/ffeltus/human-cdna-genome-blast
cd /scratch/ffeltus/human-cdna-genome-blast
```

Download the files you will need to this directory.

Here are the relevant links:
```
http://ftp.ensembl.org/pub/release-101/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
ftp://ftp.ensembl.org/pub/release-101/fasta/homo_sapiens/ncrna/Homo_sapiens.GRCh38.ncrna.fa.gz
```

Uncompress the files and ‘head’ and ‘tail’ them on the Linux command line to see if they look correct.

***Step B. Index the Genome file for BLAST***

Use a text editor (e.g. nano) to make a SLURM script (e.g. index.slurm) with the commands you need.  The top of the SLURM script are SLURM directives that tell the scheduler how many resources you need.  Below the directives, add the command lines you will need to index the FASTA genome file for BLAST. You will need to change to the directory where you will index the files and then run the makeblastdb command on the genome file. Make sure to use your username and not mine. For example (make sure each command line is on one line in your SLURM script file):
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

# Task C. BLAST Human cDNA sequences against the Human Genome
You will need blastn for this.  The cDNA hits will provide coordinates of genes in the human genome. Type the ‘blastn -h’ command for the command line parameters.

***Step A. Make a SLURM script to runthe BLAST analysis as a batch job.

Use a text editor (e.g. nano) to make a SLURM script (e.g. blastn.slurm) with the commands you need.  The top of the SLURM script are SLURM directives that tell the scheduler how many resources you need.  Below the directives, add the command lines you will need to align the human cDNA FASTA file against the reference human genome you indexed. You will need to change to the directory where you the cDNA and index files exist. Then run the blastn command on the genome file. Note: make sure each command line is on one line in your SLURM script file and use your username and directory structure.

```
#!/bin/bash
#SBATCH --job-name MAKEBLASTDB
#SBATCH --nodes 1
#SBATCH --tasks-per-node 1
#SBATCH --cpus-per-task 1
#SBATCH --mem 64gb
#SBATCH --time 48:00:00

cd /scratch/ffeltus/blast
blastn -db HG38_GENOME -query Homo_sapiens.GRCh38.ncrna.fa -outfmt 6 -evalue 1e-100 > HG38-NCRNA_HG38Genome.blastn
```

Run the pbs script using qsub (e.g. ‘qsub blastn.pbs’). The blast job will take a while (hours). Use the ‘qstat -u YOUR_USERNAME’ to see if the job is still running.  If it worked, you would see alignment data in the HG38-CDNA_HG38Genome.blastn file.

***Step B. Turn in your report in the homework folder in the Praxis LXP VM.***

Count the number of sequences in the non-coding RNA file and count the number of hits in your BLASTN output file.  (NOTE: You can count the number of sequences in a FASTA file like this: ‘cat FILE | grep ‘>’ | wc -l’ and you can count the number of one per line hits like this ‘wc -l HG38-NCRNA_HG38Genome.blastn’
Paste these into a text file in the Praxis Linux VM and copy to the homework folder in the Praxis LXP VM.  (If you need more practice, run the analysis at different E-value cutoffs and see how many hits you get … totally optional).

