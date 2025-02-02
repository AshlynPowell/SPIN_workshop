# Repeat modeling and Repeat masking

Earl Grey is a transposable element annotation pipeline. It combines both RepeatModeler and RepeatMasker, and provides almost all the output files you could possibly want. 

It's also pretty simple to run! For the full documentation see the website (https://github.com/TobyBaril/EarlGrey?tab=readme-ov-file#recommended-installation-with-conda-or-mamba)

First, we'll install and activate the environment:

```bash
module load miniconda3/4.12-pws-472
conda activate earlgrey
```

Let's create a directory to save our output to:
```bash
mkdir earl-out
```

Now we'll create a job script to run Earl Grey:

```bash
nano earlgrey.job
```

Copy and paste the job script below into your job file, editing it to match your file names, species and email.

```bash

#!/bin/bash

#SBATCH --time=72:00:00   # walltime
#SBATCH --ntasks=24   # number of processor cores (i.e. tasks)
#SBATCH --nodes=1   # number of nodes
#SBATCH --mem-per-cpu=24G   # memory per CPU core
#SBATCH -J "earl"   # job name
#SBATCH --mail-user=<youremail@email.com>   # email address
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL

# Set the max number of threads to use for programs using OpenMP. Should be <= ppn. Does nothing if the program doesn't use OpenMP.
export OMP_NUM_THREADS=$SLURM_CPUS_ON_NODE

# activate environment with Earl Grey
source ~/.bashrc
conda activate <environment>

#run Earl Grey
earlGrey -g <genome_name> -t 24 -s <species_name> -r arthropoda -d yes -o ../earl-out/
```

Start the job!

```bash
sbatch earlgrey.job
```

There are a lot of optional parameters to include in Earl Grey, we're only including a few of them:
- -t == number of threads
- -r == search term for RepeatMasker
- -d == create a soft-masked genome (we will need this later in the week, default is no)

Earl Grey usually runs for 1-3 days, depending on the size and complexity of the genome. While your job is running we're going to look at output files from a completed run. Copy over the Earl Grey output files from the shared directory:

```bash
cp -r /home/youruserid/fsl_groups/fslg_pws472/arcto_4_HiC_chrom_EarlGrey/ .
```

Move into the summaryFiles directory using cd. You'll see there are several files there, including two pdfs. Download the .pdf files to your computer. Which transposable element is found in highest frequency? Have there been any recent shifts in transposable element activity?

Check out the other files in the *summaryFiles folder using head. The .gff and .bed file provide coordinates and the contig name for each of the transposable elements. *combined_library.fasta has all of the transposable elements found in the genome. 

The hard-masked and soft-masked genomes are in a different folder. Move into *RepeatMasker. View the first few lines of each file using head <filename>. Which file is hard-masked? Which is soft-masked? 

Now we'll look at the intersect between heavy-chain fibroin and our estimated transposable elements. For this we need a .gff file with coordinates for h-fibroin. You'll find that in the shared folder here:

```bash
cp /home/youruserid/fsl_groups/fslg_pws472/compute/spin-workshop/Arcto-hfib.gff
```


